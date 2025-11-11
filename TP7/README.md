# Trabajo Práctico 7 - Pruebas de Integracion y Code Coverage

## Descripción General

Este trabajo práctico tiene como objetivo integrar diversas herramientas de aseguramiento de calidad del software en un pipeline CI/CD completo. El proyecto implementa:

- **Cobertura de código** en frontend y backend  
- **Análisis estático** mediante SonarQube Cloud  
- **Pruebas de integración end-to-end** con Cypress  
- **Pipeline de integración continua y despliegue** con Azure DevOps
- **Quality Gates automáticos** que bloquean deploys defectuosos  

El repositorio contiene el código fuente del **frontend (Next.js + Vitest + Cypress)** y del **backend (Go + go test)**, junto con la configuración del pipeline y los reportes de calidad generados.

---

## Acceso al proyecto

- Organización: **Joaquinmv03**  
- Proyecto: **TP7_IntegrationTests**  
- URL: https://dev.azure.com/joaquinmv03/TP7_IntegrationTests

La aplicación cuenta frontend en React + TypeScript + Vite y backend en Go.

--- 

## 🧱 Stack Tecnológico

| Componente                    | Tecnología                | Justificación                                                                                                                                                                   |
| ----------------------------- | ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Frontend**                  | React + Vite + TypeScript | Framework moderno, rápido y modular. Vite permite builds más veloces y Vitest se integra de forma nativa para pruebas unitarias y coverage.                                     |
| **Backend**                   | Go (Golang)               | Lenguaje compilado, eficiente y con testing integrado (`go test`). Facilita la generación de reportes de cobertura y la detección temprana de errores en tiempo de compilación. |
| **Testing Unitario Frontend** | Vitest                    | Alternativa liviana a Jest con soporte nativo para TypeScript y Vite. Genera reportes `lcov` y `cobertura.xml` compatibles con SonarCloud y Azure.                              |
| **Testing Unitario Backend**  | go test + gotestsum       | Herramientas nativas de Go para ejecutar tests con cobertura detallada (`coverage.out`, `coverage.html`).                                                                       |
| **Testing de Integración**    | Cypress                   | Permite validar flujos de usuario completos en un entorno QA real, interactuando con el backend desplegado. Ideal para pruebas end-to-end automatizadas.                        |
| **Análisis Estático**         | SonarCloud                | Solución SaaS con integración directa a Azure DevOps. Evalúa métricas de calidad, deuda técnica, duplicaciones y cobertura.                                                     |
| **CI/CD**                     | Azure DevOps Pipelines    | Proporciona un flujo unificado de integración y despliegue continuo, con soporte nativo para stages QA/PROD y conexión a SonarCloud.                                            |
| **Despliegue Cloud**          | Azure Web Apps            | Simplifica la gestión de entornos (QA y PROD) y la automatización del deploy desde pipeline.                                                                                    |


---

## Estructura del Repositorio
```yaml
TP7_IntegrationTests/
│
├── backend/
│ ├── controller/
│ ├── service/
│ ├── db/
│ ├── routes/
│ ├── coverage # coverage.out (Go)
│ ├── main.go
│ └── go.mod
│
├── frontend/
│ └── gameTracker/
│ ├── src/
│ ├── cypress/ # pruebas e2e
│ ├── coverage/ # lcov.info, cobertura.xml
│ ├── vitest.config.ts
│ └── package.json
│
├── azure-pipelines.yml
├── sonar-project.properties
├── decisiones.md
└── README.md
```
---