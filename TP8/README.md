# Trabajo Práctico 8 - Contenedores en Azure y Automatización

## Descripción General
Este tiene como objetivo demostrar la comprensión integral del ciclo de vida de aplicaciones contenidas, desde su construcción en Docker hasta su despliegue automatizado en la nube.

Se buscó implementar un enfoque cloud-agnostic y technology-agnostic, priorizando las buenas prácticas de CI/CD, separación de entornos (QA/PROD) y justificación de decisiones arquitectónicas.

El proyecto extiende la aplicación GameTracker del TP anterior, integrando ahora el uso de contenedores Docker, registry de imágenes, y pipeline automatizado que construye, publica y despliega dichas imágenes.

---

## Acceso al Proyecto

Repositorio: https://github.com/Gaallard/GameTracker

---

## Aplicación

- Frontend: React + TypeScript + Vite
- Backend: Go (Golang) + Gin + GORM
- Base de Datos: MySQL (Railway)

## Objetivos

- Contenerización completa del frontend y backend.
- Registro centralizado de imágenes (GHCR).
- Despliegue en entornos QA y PROD en la nube (Render).
- Pipeline CI/CD automatizado con testing y publicación de imágenes.

## Stack Tecnológico
| Componente             | Tecnología / Servicio            | Justificación                                                                                                                                                                 |
| ---------------------- | -------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Backend**            | Go (Golang) + Gin + GORM         | Go es rápido, concurrente y fácil de contenerizar. Gin proporciona un router eficiente, y GORM facilita el acceso a MySQL sin SQL manual. Ideal para microservicios livianos. |
| **Frontend**           | React + Vite + TypeScript        | Framework moderno y rápido. Vite acelera los builds y TypeScript mejora la calidad del código con tipado estático.                                                            |
| **Base de datos**      | MySQL (Railway)                  | DB relacional ampliamente soportada. Railway simplifica la gestión de entornos QA/PROD con credenciales separadas.                                                            |
| **Container Registry** | GitHub Container Registry (GHCR) | Integración nativa con GitHub Actions. Permite almacenar versiones de imágenes sin credenciales externas, usando el token del repositorio.                                    |
| **Hosting**            | Render                           | Plataforma simple que soporta **apps Dockerizadas** y **static sites**. Permite separar QA y PROD con configuraciones independientes.                                         |
| **CI/CD**              | GitHub Actions                   | Integración directa con GitHub. Permite correr tests, build de imágenes, push al registry y auto-deploy a Render.                                                             |


## Estructura del Proyecto
```yaml
GameTracker/
│
├── backend/
│   ├── controller/
│   ├── service/
│   ├── models/
│   ├── Dockerfile
│   ├── main.go
│   └── go.mod
│
├── frontend/
│   └── gameTracker/
│       ├── src/
│       ├── public/
│       ├── Dockerfile
│       └── package.json
│
├── .github/
│   └── workflows/
│       └── ci.yml
│
└── README.md
```
