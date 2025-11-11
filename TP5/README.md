# Trabajo Práctico 5 - DevOps CICD Pipelines 

## Acceso al proyecto

- Organización: **Joaquinmv03**  
- Proyecto: **TP4_Pipeline**  
- URL: https://dev.azure.com/joaquinmv03/TP4_Pipeline

La aplicación cuenta frontend en React + TypeScript + Vite y backend en Go.

--- 

## Ejecución Local 

Este proyecto está organizado en dos carpetas principales:
- /frontend/gameTracker 
- /backend 

Este proyecto implementa una arquitectura **CI/CD completa** utilizando **Azure DevOps Pipelines** para una aplicación full stack compuesta por:

- **Backend:** API REST en Go.
- **Frontend:** Aplicación web en React (Vite).
- **Infraestructura Cloud:** Azure App Service, para QA y PROD, ambos ambientes separados.

El objetivo fue automatizar la compilación, empaquetado, despliegue y validación de ambos componentes, aplicando prácticas de integración continua y despliegue continuo (CI/CD).

## Entornos y URLs de despliegue

### **Ambiente QA**
- Backend (Go): https://webapp-tp05-back-qa.azurewebsites.net/ 
- Frontend (React): https://webapp-tp05-front-qa.azurewebsites.net/

### **Ambiente PROD**
- Backend (Go): https://webapp-tp05-back-prod.azurewebsites.net/  
- Frontend (React): https://webapp-tp05-front-prod.azurewebsites.net/

---

## Proceso de Despliegue - Pipeline en Azure DevOps

El pipeline está definido en azure-pipelines.yml y se ejecuta automáticamente en cada push a main.

### CI (Integración Continua)

En esta primera etapa se construyen ambos componentes del sistema:

#### Frontend (React)

- Se instalan las dependencias con npm ci.

- Se genera la versión final del sitio con npm run build.

- El resultado (carpeta dist) se guarda como artefacto para usarlo en el despliegue.

### Backend (Go)

- Se ordenan las dependencias con go mod tidy.

- Se ejecutan los tests (go test).

- Se compila el backend para Linux (go build), generando un archivo ejecutable llamado server.

- Se incluye un archivo startup.sh que se encarga de iniciar el programa en Azure.

- Todo se guarda como artefacto para el despliegue.

Se generan dos artefactos:

**front_dist → contiene el frontend listo para subir.**
**back_bin → contiene el backend compilado y el script de inicio.**

### CD (Deploy Continua)

#### Deploy QA

Esta etapa toma los artefactos generados y los despliega en las Web Apps de QA:

- Se descarga lo generado en la etapa anterior.

- Se empaqueta el backend (binario + startup.sh) en un archivo ZIP.

- Se sube ese ZIP al App Service de Azure para el backend de QA.

- Se despliega el frontend directamente desde la carpeta dist.

- Se realizan verificaciones de salud (health checks):

-- Backend → verifica que responda correctamente el endpoint /health.

-- Frontend → verifica que la página principal (/) se cargue bien.


#### Deploy PROD

Una vez que QA funciona bien, se despliega la misma versión en producción, pero con aprobación manual:

- Descarga artefactos.

- Empaqueta y despliega backend.

- Despliega frontend.

- Ejecuta los health checks.

## Decisiones principales

- **Separación QA y PROD:** permite probar antes de publicar en producción.

- **startup.sh:** es necesario para que Azure ejecute el backend en Linux correctamente.

- **Health checks:** aseguran que la app funcione antes de avanzar.

- **Despliegue automático:** al subir cambios a main, el sistema realiza todo el proceso sin intervención manual, salvo la aprobación final para producción.