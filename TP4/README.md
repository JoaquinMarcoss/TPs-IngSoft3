# Trabajo Práctico 4 - Azure DevOps Pipelines

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

#### Frontend
Los comandos son los siguientes: 
- cd frontend/gameTracker
- npm install
- npm run dev

La aplicación va a abrir en: http://localhost:5173

#### Backend
Los comandos son los siguientes: 
- cd backend
- go mod tidy
- go run main.go

El backend se va a quedar escuchando en: http://localhost:8080 

---

## Pipeline en Azure DevOps

El pipeline está definido en azure-pipelines.yml, el mismo se ejecuta automáticamente en cada push a main.

**Stages principales**
- Frontend → npm ci && npm run build → publica dist.
- Backend → go build → publica binario en bin/.

Publica artefactos: front_dist y back_bin.

--- 

## Requisitos del agente Self-Hosted

- Node.js >= 20.x
- npm >= 10.x
- Go = 1.22.1