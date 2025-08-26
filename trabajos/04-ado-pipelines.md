# ⚠️ IMPORTANTE – Guía de Práctica Sugerida

Lo que vas a ver a continuación es una **guía paso a paso altamente sugerida** para que practiques el uso de Azure DevOps Pipelines.  
**Te recomendamos hacerla completa**, ya que te ayudará a adquirir los conocimientos necesarios.

---

## PERO: Esta guía **NO es el trabajo práctico** que tenés que entregar

El trabajo práctico será evaluado en base a:
- Tu capacidad para **configurar y utilizar Azure Pipelines con criterio técnico**.
- Tu capacidad para **explicar y justificar cada decisión que tomaste**.
- Una **defensa oral obligatoria** donde vas a tener que demostrar lo que sabés.

---

## ¿Dónde está el trabajo práctico?

El **TP real que debés entregar y defender** se encuentra al final de este archivo.  
No alcanza con copiar esta guía. **Si no podés defenderlo, no se aprueba.**

---

## Sobre esta guía

- Esta guía NO es exhaustiva.  
- Azure DevOps Pipelines requiere **investigación y práctica fuera de clase**.  
- En 2 horas no vas a aprenderlo completo. **Esto es solo el punto de partida.**

---

# Guía Paso a Paso – Azure DevOps Pipelines (Práctica sugerida)

## 1- Objetivos de Aprendizaje
- Adquirir conocimientos acerca de las herramientas de integración y entrega continua en ADO.
- Configurar pipelines **como código** con YAML.
- Implementar procesos de **build, test, empaquetado, publicación y despliegue**.
- Ejecutar despliegues en **agentes Self‑Hosted**.

## 2- Algunos conceptos fundamentales

### CI/CD en contexto
- **Continuous Integration (CI):** integrar código frecuentemente, ejecutar pruebas y validar el estado del repo.  
- **Continuous Delivery (CD):** automatizar preparación y verificación para despliegues frecuentes, con aprobaciones si aplica.  
- **Continuous Deployment:** automatizar hasta producción cuando todas las validaciones pasan.

### ¿Qué es Azure DevOps Pipelines?
- Servicio de **CI/CD** de Microsoft para compilar, testear y desplegar en múltiples entornos.  
- Definición como código con **YAML**, versionado en el repo.  
- Soporta **Microsoft‑hosted** y **Self‑Hosted agents**.

### Agentes
- **Microsoft‑hosted:** cero mantenimiento, menor personalización de SO y herramientas.  
- **Self‑Hosted:** control total (SO, SDKs, Docker, DB, puertos), ideal para despliegues locales o entornos restringidos.

---

# Trabajo Práctico 04 – Azure DevOps Pipelines (2025)

## 🎯 Objetivo

Construir un **pipeline CI/CD en YAML** para una aplicación **a elección** que tenga **frontend, backend y base de datos**, y **desplegarla en tu máquina** usando un **agente Self‑Hosted**.

Este trabajo se aprueba **solo si podés explicar qué hiciste, por qué lo hiciste y cómo lo resolviste**.

---

## 🧩 Escenario (actualizado)

Como líder técnico, debés:
1. Elegir una app (o crear una mínima) con **Front + Back + DB** (stack libre: Angular/React/Vue + .NET/Node/Java + SQL/Postgres/MySQL, etc.).  
2. Versionar todo en **un único repo** (mono‑repo recomendado) con carpetas `/front`, `/back`, `/db`.  
3. Definir un **pipeline multi‑stage en YAML** con **CI** (build+test) y **CD** (deploy) que **corra el despliegue en un agente Self‑Hosted** instalado en tu equipo.  
4. El despliegue puede orquestarse con **Docker Compose** o **scripts nativos** (systemd/npm/dotnet/java), pero debe incluir:  
   - Construcción/obtención de artefactos.  
   - **Migraciones/seed** de la base de datos (EF Core, Flyway, Liquibase o script SQL).  
   - **Variables/secretos** gestionados correctamente (Variable Groups/Library, `.env`, Key Vault si aplica).  
   - **Health checks** y verificación post‑deploy.  
   - **Plan de rollback** básico.

---

## 📋 Tareas que debés cumplir

### 1. Preparación del entorno
- Crear **Pool** y **Agente Self‑Hosted** en ADO (instalado como servicio en tu máquina).  
- Documentar requisitos (SDKs, Node, Docker, DB local, puertos) y **capabilities** del agente.

### 2. Estructura del repo y definición del pipeline
- Organizar `/front`, `/back`, `/db` y agregar **`azure-pipelines.yml`** en raíz.  
- YAML requerido (multi‑stage):
  - **Stage CI** (trigger en `main`):  
    - Build front (por ejemplo `npm ci && npm run build`).  
    - Build back (por ejemplo `dotnet restore/build/test` o `mvn package`/`gradle build`).  
    - Tests automáticos (front y back).  
    - Publicación de artefactos (dist/bin, scripts de DB).  
  - **Stage CD** (deployment):  
    - Job de **deployment** apuntando al **pool self‑hosted**.  
    - Paso de migraciones/seed de DB.  
    - Despliegue de back y front (Docker Compose o scripts).  
    - Health check (curl o script) y salida clara de éxito/fallo.

### 3. Gestión de secretos y configuración
- No subir credenciales al repo.  
- Usar **Variables**/Library y (opcional) `.env` solo en la máquina del agente.  
- Documentar en `decisiones.md` cómo protegés credenciales y configuraciones.

### 4. Estrategia de rollback
- Describir y **automatizar mínimamente** un rollback (p.ej., `docker compose down && docker compose up` con imagen previa, o restaurar release anterior de artefactos).

### 5. Evidencias
- Capturas: creación del pool/agente, ejecuciones de CI y CD, artefactos publicados, consola del despliegue, health check OK y app corriendo.

---

## 🔧 Pasos sugeridos (checklist)

1. **Self‑Hosted Agent**
   - Crear Pool `SelfHosted` y registrar `Agent-Local` (como servicio).  
2. **Repo**
   - Estructura `/front`, `/back`, `/db`, `azure-pipelines.yml`.  
3. **CI**
   - Build+test front y back, publicar artefactos.  
4. **CD**
   - Deployment job -> pool `SelfHosted`.  
   - Migraciones DB + despliegue (Compose o scripts).  
   - Health check.  
5. **Rollback**
   - Script/compose para volver a versión anterior.  
6. **Evidencias**
   - Capturas y explicación en `decisiones.md`.

---

## 📄 Entregables

1. **Acceso al proyecto en Azure DevOps** con:
   - Pipeline **YAML** multi‑stage (CI + CD) apuntando al **Self‑Hosted agent**.  
   - Ejecuciones exitosas (logs visibles) y artefactos publicados.

2. **Repositorio en GitHub** con:
   - **README.md**: cómo ejecutar local, cómo corre el pipeline, prerequisitos del agente, puertos, URLs y health checks.  
   - **decisiones.md** con:  
     - Stack elegido y estructura del repo.  
     - Diseño del pipeline (stages, jobs, artefactos).  
     - Gestión de variables/secretos.  
     - Estrategia de rollback.  
     - Evidencias (capturas).  
   - (Opcional) `docker-compose.yml` y scripts de migración en `/db`.

3. **URL del proyecto** en la planilla:  
   - [Planilla de TPs](https://docs.google.com/spreadsheets/d/1mZKJ8FH390QHjwkABokh3Ys6kMOFZGzZJ3-kg5ziELc/edit?gid=0#gid=0)

---

## 🗣️ Defensa Oral Obligatoria

Preguntas típicas:
- ¿Por qué YAML y no Classic para este caso?  
- ¿Cómo garantizás reproducibilidad entre CI y CD?  
- ¿Cómo aislaste secretos? ¿Qué alternativas consideraste?  
- ¿Qué valida tu health check y cómo decidís fallar un deploy?  
- ¿Cómo ejecutás y revertís migraciones de DB?

---

## ✅ Evaluación

| Criterio                                                    | Peso |
|-------------------------------------------------------------|------|
| Pipeline YAML multi‑stage (CI + CD) funcionando             | 25%  |
| Despliegue en Self‑Hosted con migraciones y health check    | 25%  |
| Claridad y justificación en `decisiones.md`                 | 25%  |
| Defensa oral: comprensión y argumentación                   | 25%  |

---

## ⚠️ Uso de IA

Podés usar IA (ChatGPT, Copilot), pero **deberás declarar qué parte fue generada con IA** y justificar cómo la verificaste.  
Si no podés defenderlo, **no se aprueba**.

---

## 📎 Anexo: plantilla mínima de `azure-pipelines.yml` (orientativa)

> Adaptá comandos a tu stack. Si usás Docker Compose para CD, instalalo en el agente y versioná `docker-compose.yml`.

```yaml
trigger:
  branches:
    include:
      - main

stages:
  - stage: CI
    displayName: Build & Test
    jobs:
      - job: build_test
        displayName: Build Front/Back + Test
        pool:
          vmImage: 'ubuntu-latest'   # Podés usar MS-hosted en CI y Self-Hosted en CD, o todo Self-Hosted
        steps:
          - checkout: self

          # Frontend
          - task: NodeTool@0
            inputs:
              versionSpec: '20.x'
          - script: |
              cd front
              npm ci
              npm run build
            displayName: Build Front

          # Backend (ejemplos .NET o Node/Java: cambiá según tu stack)
          - task: UseDotNet@2
            inputs:
              packageType: 'sdk'
              version: '8.0.x'
          - script: |
              cd back
              dotnet restore
              dotnet build --configuration Release
              dotnet test --configuration Release --no-build
            displayName: Build & Test Back

          # Publicar artefactos (dist y binarios + scripts DB)
          - task: PublishBuildArtifacts@1
            inputs:
              PathtoPublish: '$(Build.SourcesDirectory)'
              ArtifactName: 'drop'
              publishLocation: 'Container'

  - stage: CD
    displayName: Deploy Local (Self-Hosted)
    dependsOn: CI
    jobs:
      - deployment: deploy_local
        displayName: Deploy en agente local
        environment: 'Local.SelfHosted'
        strategy:
          runOnce:
            deploy:
              steps:
                - download: current
                  artifact: drop

                # Opcional: cargar variables/secretos desde archivos locales del agente (no versionados)
                - script: |
                    echo "Cargando variables locales (.env) si corresponde"
                  displayName: Preparar variables

                # Migraciones de base de datos (adaptar a EF/Flyway/Liquibase/SQL)
                - script: |
                    echo "Aplicando migraciones de DB..."
                    # ejemplo EF Core:
                    # cd back && dotnet tool restore && dotnet ef database update --project ./Back.csproj
                  displayName: Migraciones DB

                # Despliegue (Docker Compose o scripts)
                - script: |
                    echo "Desplegando servicios..."
                    # docker compose down && docker compose up -d --build
                    # o: pm2/systemd/npm start/dotnet run, etc.
                  displayName: Desplegar app

                # Health check
                - script: |
                    echo "Chequeando salud..."
                    # curl -f http://localhost:8080/health || exit 1
                  displayName: Health Check
        pool:
          name: 'SelfHosted'   # Tu pool local
```
