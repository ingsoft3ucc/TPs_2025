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
- Implementar procesos de **build, empaquetado, publicación y despliegue**.
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

Construir un **pipeline CI/CD en YAML** para una aplicación **a elección** que tenga **frontend y backend**, y **desplegarla en tu máquina** usando un **agente Self‑Hosted**.

Este trabajo se aprueba **solo si podés explicar qué hiciste, por qué lo hiciste y cómo lo resolviste**.

---

## 🧩 Escenario (actualizado)

Como líder técnico, debés:
1. Elegir una app (o crear una mínima) con **Front + Back** (stack libre: Angular/React/Vue + .NET/Node/Java, etc.).  
2. Versionar todo en **un único repo** (mono‑repo recomendado) con carpetas `/front`, `/back`.  
3. Definir un **pipeline multi‑stage en YAML** con **CI** (build+test) y **CD** (deploy) que **corra el despliegue en un agente Self‑Hosted** instalado en tu equipo.  

---

## 📋 Tareas que debés cumplir

### 1. Preparación del entorno
- Crear **Pool** y **Agente Self‑Hosted** en ADO (instalado como servicio en tu máquina).  

### 2. Estructura del repo y definición del pipeline
- Organizar `/front`, `/back`  y agregar **`azure-pipelines.yml`** en raíz.  
- YAML requerido (multi‑stage):
  - **Stage CI** (trigger en `main`):  
    - Build front (por ejemplo `npm ci && npm run build`).  
    - Build back (por ejemplo `dotnet restore/build/test` o `mvn package`/`gradle build`).  
    - Publicación de artefactos (dist/bin).  
  - **Stage CD** (deployment):  
    - Job de **deployment** apuntando al **pool self‑hosted**.  
    - Despliegue de back y front.  
    
### 3. Evidencias
- Capturas: creación del pool/agente, ejecuciones de CI y CD, artefactos publicados, consola del despliegue y app corriendo.

---

## 🔧 Pasos sugeridos (checklist)

1. **Self‑Hosted Agent**
   - Crear Pool `SelfHosted` y registrar `Agent-Local` (como servicio).  
2. **Repo**
   - Estructura `/front`, `/back`,  `azure-pipelines.yml`.  
3. **CI**
   - Build+test front y back, publicar artefactos.  
4. **CD**
   - Deployment job -> pool `SelfHosted`.   
5. **Evidencias**
   - Capturas y explicación en `decisiones.md`.

---

## 📄 Entregables

1. **Acceso al proyecto en Azure DevOps** con:
   - Pipeline **YAML** multi‑stage (CI + CD) apuntando al **Self‑Hosted agent**.  
   - Ejecuciones exitosas (logs visibles) y artefactos publicados.

2. **Repositorio en GitHub** con:
   - **README.md**: cómo ejecutar local, cómo corre el pipeline, prerequisitos del agente, puertos, URLs .  
   - **decisiones.md** con:  
     - Stack elegido y estructura del repo.  
     - Diseño del pipeline (stages, jobs, artefactos).  
     - Evidencias (capturas).  
  
3. **URL del proyecto** en la planilla:  
   - [Planilla de TPs](https://docs.google.com/spreadsheets/d/1mZKJ8FH390QHjwkABokh3Ys6kMOFZGzZJ3-kg5ziELc/edit?gid=0#gid=0)

---

## 🗣️ Defensa Oral Obligatoria

Preguntas típicas:
- ¿Por qué YAML y no Classic para este caso?  
- ¿Cómo garantizás reproducibilidad entre CI y CD?  
- ¿Cómo aislaste secretos? ¿Qué alternativas consideraste?  
- ¿Cómo ejecutarías  migraciones de DB?

---

## ✅ Evaluación

| Criterio                                                    | Peso |
|-------------------------------------------------------------|------|
| Pipeline YAML multi‑stage (CI + CD) funcionando             | 25%  |
| Despliegue en Self‑Hosted                                   | 25%  |
| Claridad y justificación en `decisiones.md`                 | 10%  |
| Defensa oral: comprensión y argumentación                   | 40%  |

---

## ⚠️ Uso de IA

Podés usar IA (ChatGPT, Copilot), pero **deberás declarar qué parte fue generada con IA** y justificar cómo la verificaste.  
Si no podés defenderlo, **no se aprueba**.
