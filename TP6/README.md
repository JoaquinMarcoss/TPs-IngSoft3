# Trabajo Práctico 6 - Pruebas Unitarias

Este documento explica cómo ejecutar las pruebas unitarias del proyecto GameTracker, tanto en el backend (Go) como en el frontend (React/TypeScript), junto con los prerrequisitos y comandos principales.

## Acceso al proyecto

- Organización: **Joaquinmv03**  
- Proyecto: **TP6_UnitTests**  
- URL: https://dev.azure.com/joaquinmv03/TP6_UnitTests

La aplicación cuenta frontend en React + TypeScript + Vite y backend en Go.

## Prerrequisitos
### Backend (Go)

- Go 1.22+
- gotestsum y go-junit-report (para reportes)

**Dependencias instaladas:**
- go mod tidy

### Frontend (React/TypeScript)
- Node.js 20+
- npm o pnpm

**Dependencias instaladas:**
- cd frontend/gameTracker
- npm ci

## Ejecución de Pruebas
### Backend (Go)
#### 1. Ejecutar pruebas unitarias
```yaml
 cd backend
 gotestsum --format=standard-verbose --junitfile test-results-go.xml -- \
  -covermode=atomic -coverprofile=coverage.out ./...
```

#### 2. Generar reportes de cobertura
```yaml
- go tool cover -func=coverage.out > coverage.txt
- go tool cover -html=coverage.out -o coverage.html
```

#### 3. Verificar cobertura mínima del 70%
```yaml
 awk '/total:/ { gsub("%","",$3); if ($3+0 < 70) { print "Coverage below 70%: " $3"%"; exit 1 } else { print "Coverage: " $3"%" } }' coverage.txt
```

### Frontend (React/TypeScript)
#### 1. Ejecutar pruebas unitarias
```yaml
cd frontend/gameTracker
npm run test
```

#### 2. Ejecutar pruebas con cobertura
```yaml
npm run coverage
```

#### 3. Generar reportes de cobertura
Los reportes se generan automáticamente en la carpeta:

- frontend/gameTracker/coverage/

#### 4. Resultados esperados

**coverage/index.html →** reporte visual

**test-results-vue.xml →** resultados en formato JUnit (para CI/CD)

## Reportes de Cobertura
### Backend

**- coverage.out →** archivo principal de cobertura

**- coverage.txt →** resumen en texto

**- coverage.html →** reporte visual interactivo

### Frontend

**- /coverage/index.html →** cobertura visual

**- test-results-vue.xml →** reporte JUnit para pipelines

## Estructura del Proyecto
```yaml
GameTracker/
├── backend/
│   ├── service/
│   ├── controller/
│   ├── run-tests.sh
│   ├── coverage.html
│   └── test-results-go.xml
├── frontend/gameTracker/
│   ├── src/components/__tests__/
│   ├── src/services/__tests__/
│   ├── coverage/
│   └── test-results-vue.xml
└── .github/workflows/tests.yml
```
