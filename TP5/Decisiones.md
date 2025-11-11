# Trabajo Práctico 5 - DevOps CICD Pipelines

- Acceso al proyecto: https://dev.azure.com/joaquinmv03/TP4_Pipeline

--- 

## Cloud Resources

Se implementaron los siguientes recursos principales:

**App Service (QA y PROD):** Aloja el backend desarrollado en Go.
**App Service (QA y PROD):** Aloja el frontend construido con React.
**Azure Database for MySQL** Base de datos utilizada por el backend para almacenar información.

Cada componente cuenta con su propio **App Service** para mantener separados los entornos de QA y PROD, lo que permite probar los cambios antes de llevarlos al entorno final.

La creación de las Web Apps fue de la siguiente manera: 

![Web App QA](./img/backQA1.jpg)

![Web App QA](./img/backQA2.jpg)

![Web App QA](./img/backQA3.jpg)

![Web App QA](./img/backQA4.jpg)

![Web App QA](./img/backQA5.jpg)

Quedando: 

![Web App QA](./img/resumenbackQA.jpg)

De la misma forma creamos la Web App para el Front de QA:

![Web App QA](./img/resumenfrontQA.jpg)

Ahora para crear las Web Apps de PROD vamos a usar Template Deployment, para implementar las plantillas para la automatización que descargamos de las Web Apps de QA. Para eso seguimos los siguientes pasos: 

- Creamos nuestra propia plantilla en el editor
- Descomprimimos el archivo template.zip que decargamos.
- Cargamos el archivo template.json que se extrajo del archivo template.zip.

![Web App PROD](./img/templatedeploymentbackPROD.jpg)

- Guardamos y editamos parámetros.

![Web App PROD](./img/templatedeploymentbackPROD2.jpg)

- Cargamos el archivo parameters.json que se extrajo del archivo template.zip.
- Guardamos y editamos parámetros para cambiar el nombre de nuestra Web App agregandole el sufijo "-PROD".

Quedando: 

![Web App PROD](./img/templatedeploymentbackPROD3.jpg)

De misma manera y con los mismo pasos el Front: 

![Web App PROD](./img/templatedeploymentfrontPROD.jpg)

Quedando asi el grupo de recursos: 

![Creacion BD](./img/recursos1.jpg)

Para las bases de datos hacemos lo siguiente, primero para QA: 

![Creacion BD](./img/bd1.jpg)
![Creacion BD](./img/bd2.jpg)
![Creacion BD](./img/bd3.jpg)
![Creacion BD](./img/bd4.jpg)
![Creacion BD](./img/bd6.jpg)

Quedando: 

![Creacion BD](./img/resumenbdQA.jpg)

Los mismos pasos para la base de datos de PROD.

![Creacion BD](./img/resumenbdPROD.jpg)

Ahora con las bases de datos creadas debemos realizar las siguiente configuraciones. Primero para ingresar utilizamos los siguiente comandos: 
- mysql -h mysql-tp05-qa.mysql.database.azure.com -u adminqa -p --ssl
- mysql -h mysql-tp05-prod.mysql.database.azure.com -u adminprod -p --ssl

![Creacion BD](./img/bdconfig.jpg)

Luego dentro, creamos las bases de datos para nuestro proyecto: 
```yaml
CREATE DATABASE IF NOT EXISTS gametracker_qa
  CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

CREATE USER IF NOT EXISTS 'qa_user'@'%' IDENTIFIED BY 'Qa#2025!strong';
GRANT ALL PRIVILEGES ON gametracker_qa.* TO 'qa_user'@'%';
FLUSH PRIVILEGES;
```
```yaml
CREATE DATABASE IF NOT EXISTS gametracker_prod
  CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

CREATE USER IF NOT EXISTS 'prod_user'@'%' IDENTIFIED BY 'Prod#2025!strong';
GRANT ALL PRIVILEGES ON gametracker_prod.* TO 'prod_user'@'%';
FLUSH PRIVILEGES;
```
Por ultimo configuramos la Service Connection en Azure DevOps: 

![service connection](./img/serviceconnection.jpg)

![service connection](./img/serviceconnection2.jpg)
---

## Release Pipeline

El pipeline fue diseñado en el archivo _azure-pipelines.yml_ y se ejecuta automáticamente en cada push a la rama **main**.

**Etapas principales:**

### 1. CI (Integración Continua) 
   - Compila y genera artefactos del backend (Go) y frontend (React).  
   - Publica los artefactos:
     - `front_dist` (frontend listo para desplegar).  
     - `back_bin` (binario y script de inicio del backend).

### 2. Deploy QA 
   - Descarga los artefactos generados.  
   - Empaqueta el backend en un ZIP (binario + `startup.sh`).  
   - Despliega backend y frontend en sus Web Apps de QA.  
   - Realiza health checks automáticos.

### 3. Deploy PROD  
   - Se ejecuta luego de QA si todo fue exitoso.  
   - Despliega la misma versión validada en producción.  
   - Incluye una aprobación manual antes de avanzar.

Tambien tuvimos que agregar un script **startup.sh**  necesario para que Azure ejecute el binario Go dentro del entorno Linux:

![startup.sh](./img/startupsh.jpg)

---

## Variables y Secrets

Las variables y secretos se configuraron desde Azure DevOps y Azure App Service.

### Variables en el pipeline
Definidas en la parte superior del `azure-pipelines.yml`:
- `azureSubscription`: conexión con la suscripción de Azure.
- `qaWebAppBack`, `qaWebAppFront`: nombres de las Web Apps de QA.
- `prodWebAppBack`, `prodWebAppFront`: nombres de las Web Apps de Producción.

### Secrets en Azure App Service
Definidos en la configuración del backend (App Settings):
- `MYSQL_HOST`, `MYSQL_USER`, `MYSQL_PASSWORD`, `MYSQL_DB`: datos de conexión a la base MySQL.  
- `PORT`: puerto donde escucha el servicio (por defecto `8080`).  
- `ALLOWED_ORIGINS`: para controlar los orígenes permitidos por CORS.  

![varibles de entorno](./img/bdconfig2.jpg)

![varibles de entorno](./img/bdconfig3.jpg)

---

## ✅ Aprobaciones

El pipeline utiliza un flujo controlado con aprobación manual antes del despliegue en Producción.

- La etapa **CI + Deploy QA** se ejecuta automáticamente al hacer push a `main`.
- La etapa **Deploy PROD** requiere aprobación desde el entorno PROD en Azure DevOps, donde solo los usuarios autorizados pueden aprobar el paso a producción: 

![varibles de entorno](./img/PRODapprival.jpg)

![varibles de entorno](./img/PRODapprival2.jpg)

---

## Health Checks

En cada despliegue (QA y PROD), el pipeline realiza verificaciones automáticas para confirmar que las aplicaciones funcionan correctamente:
- **Backend (Go)** : donde `/health` devuelve `{ "status": "ok" }` con código 200.
- **Frontend (React)** se hace `/` que responde el HTML principal (`<!DOCTYPE html>`).

Si alguno falla, el pipeline se detiene y marca error, evitando avanzar a producción con una versión defectuosa.

---

## Evidencias y pruebas

![varibles de entorno](./img/pipelinefinal.jpg)

![back QA](./img/backqa.jpg)

![front QA](./img/frontqa.jpg)

![back PROD](./img/backprod.jpg)

![front PROD](./img/pipelinefinal.jpg)
