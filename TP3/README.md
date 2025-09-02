# Trabajo Práctico 3 - Introducción a Azure DevOps

## Acceso al proyecto

- Organización: **Joaquinmv03**  
- Proyecto: **GameTracker**  
- URL: https://dev.azure.com/joaquinmv03/GameTracker

---

## Clonar y trabajar con el repositorio

Para poder trabajar con el repositorio lo que debemos hacer es clonar desde Azure Repos el repositorio __GameTracker__ utilizando el siguiente comando: 

- git clone https://joaquinmv03@dev.azure.com/joaquinmv03/GameTracker/_git/GameTracker

Una vez realizado eso vamos a poder trabajar con las distintas ramas, crear nuevas, realizar cambios, hacer commits y pushearlos. 

--- 

## Estructura del proyecto

GameTracker/
├── backend/               
│   ├── controller
│   ├── service
│   └── ...
├── frontend/gameTracker/  
│   ├── src
│   └── ...   
└── README.md        

El proyecto consta en dos partes principales:

**Backend (Go):** Lógica, conexión a la base de datos y endpoints de la API.

- controller/ → recibe las peticiones.
- service/ → contiene la lógica del negocio.
- models/ → define las estructuras de datos.
- routes/ → agrupa y configura las rutas.
- db/ → gestiona la base de datos.
- main.go → archivo principal que levanta el servidor.

**Frontend (React/TypeScript):** Interfaz web que del usuario.

- public/ → archivos estáticos (ej. index.html).
- src/ → código fuente de la aplicación (componentes, páginas, etc.).

