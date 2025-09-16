# Trabajo Práctico 2 -  Introducción a Docker

## Acceso al proyecto

- Repo: https://github.com/Gaallard/GameTracker

---

## Construcción de imágenes

Para la aplicación se crearon dos Dockerfile, uno para backend y otro para el frontend. Luego de estructurar los archivos, para poder crear una imagen etiquetándola con un nombre ejecutamos el siguiente comando. El punto final le indica a Docker que use el directorio actual:

- docker build -t gametracker-backend .
- docker build -t gametracker-frontend .

Luego, para etiquetar las imágenes con nuestro usuario de Docker Hub y un tag significativo, realizamos lo siguiente: 

- docker tag gametracker-backend gaallardddd/gametracker-backend:v1.0
- docker tag gametracker-frontend gaallardddd/gametracker-frontend:v1.0

---

# Ejecutar contenedores

Si bien la ejecucion de contenedores usamos Docker Desktop, Para la ejecución desde la terminal, usamos los siguientes comandos: 
- docker run 

Para ver los contenedores ejecutados utilizando el comando ps:
- docker ps

Y en caso de que este nada en ejecución, usamos el siguiente para ver los contenedores que tenemos:
- docker ps -a

---

## Acceder a la aplicación y base de datos (URLs, puertos)

#### QA
- Web app: http://localhost:3000 (mapea 3000:80)
- API: http://localhost:8081 (mapea 8081:8080)
- Base de datos: localhost, puerto 3307

#### PROD
- Web app: http://localhost:8080 ← (mapea 8080:80)
- API: http://localhost:8082 ← (mapea 8082:8080)
- Base de datos: localhost, puerto 3308

---

## Verificar que todo funciona correctamente

Ademas de usar la UI de Docker Desktops para la corroboración, algunos comandos para verificar el funcionamiento es el siguiente: 
**levantar todo en segundo plano**
- docker compose up -d
**ver estado y puertos mapeados**
- docker compose ps

Tambien se puede utilizar el comando docker inspect que sirve para ver health, puertos, IP, volúmenes, variables, estado, etc. Algunos ejemplos son: 
**Estado: running/exited y exit code**
- docker inspect -f '{{.State.Status}} (exit={{.State.ExitCode}})' gametracker_backend_qa

**Health de MySQL QA/PROD**
- docker inspect -f '{{.State.Health.Status}}' gametracker_db_qa
- docker inspect -f '{{.State.Health.Status}}' gametracker_db_prod 

