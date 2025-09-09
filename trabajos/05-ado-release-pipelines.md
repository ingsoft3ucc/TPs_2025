# ⚠️ IMPORTANTE – Guía de Práctica Sugerida

Lo que vas a ver a continuación es una **guía paso a paso altamente sugerida** para que practiques el uso de Azure DevOps Release Pipelines.  
**Te recomendamos hacerla completa**, ya que te ayudará a adquirir los conocimientos necesarios.

---

## PERO: Esta guía **NO es el trabajo práctico** que tenés que entregar

El trabajo práctico será evaluado en base a:
- Tu capacidad para **configurar y utilizar Azure Release Pipelines con criterio técnico**.
- Tu capacidad para **explicar y justificar cada decisión que tomaste**.
- Una **defensa oral obligatoria** donde vas a tener que demostrar lo que sabés.

---

## ¿Dónde está el trabajo práctico?

El **TP real que debés entregar y defender** se encuentra al final de este archivo.  
No alcanza con copiar esta guía. **Si no podés defenderlo, no se aprueba.**

---

## Sobre esta guía

- Esta guía NO es exhaustiva.  
- Azure DevOps Release Pipelines requiere **investigación y práctica fuera de clase**.  
- En 2 horas no vas a aprenderlo completo. **Esto es solo el punto de partida.**

## 📊 Presentación Resumida

**[Ver presentación de conceptos clave →](https://gamma.app/docs/Azure-DevOps-Release-Pipelines-pbfw9eemok5pjze)**

Esta presentación resume los conceptos principales que vas a ver en esta guía práctica.

---

# Guía Paso a Paso – Azure DevOps Release Pipelines (Práctica sugerida)

## 1- Objetivos de Aprendizaje
- Adquirir conocimientos acerca de las herramientas de despliegue y releases de aplicaciones.
- Configurar este tipo de herramientas.
- Comprender el concepto de recurso en Azure
- Comprender los conceptos básicos de Release Pipelines en Azure DevOps.
- Configurar un Release Pipeline para automatizar despliegues en diferentes entornos.

## 2- Algunos conceptos fundamentales

### CI vs CD en Pipelines YAML
- **Build (CI):** enfocado en **integración continua**, compilación, testing y generación de artefactos.  
- **Deploy (CD):** enfocado en **entrega/despliegue continuo**, distribución de artefactos a diferentes entornos.  
- **Entornos múltiples:** QA, Staging, Production con diferentes configuraciones y aprobaciones.
- **Pipeline único:** Un solo pipeline YAML maneja tanto CI como CD con múltiples stages.

### ¿Por qué YAML Pipelines en lugar de Release Pipelines clásicos?
- Los **Release Pipelines clásicos están discontinuados** y Microsoft recomienda migrar a YAML.
- **Pipeline as Code**: configuración versionada en el repositorio junto al código.
- **Mayor flexibilidad**: lógica condicional, loops, variables complejas y templates reutilizables.
- **Environments**: control de **aprobaciones** y **gates** entre entornos directamente en YAML.
- **Deployment jobs**: estrategias avanzadas (blue/green, canary, rolling) nativamente soportadas.

### Entornos y Estrategias

#### Progresión de Entornos
- **Desarrollo (DEV)**: Entorno para desarrollo activo, cambios constantes, base de datos local/compartida básica.
- **QA (Quality Assurance)**: Entorno estable para testing automatizado y manual, datos de prueba consistentes.
- **Staging (STG)**: Réplica exacta de producción, testing final pre-release, datos similares a producción.
- **Producción (PROD)**: Entorno live con usuarios reales, máxima estabilidad, datos reales críticos.


### Estructura de Pipelines YAML

#### ¿Qué es un Pipeline?
Un **pipeline** es un conjunto automatizado de procesos que se ejecutan para llevar el código desde el desarrollo hasta producción.

#### Componentes básicos:

**Stage (Etapa):**
- Agrupa trabajos relacionados (ej: Build, Test, Deploy)
- Se ejecutan secuencialmente por defecto
- Representan fases principales del proceso

**Job (Trabajo):**
- Conjunto de pasos que se ejecutan en una máquina virtual
- Varios jobs en un stage se ejecutan en paralelo
- Ejemplo: compilar código, ejecutar tests

**Task/Step (Tarea/Paso):**
- Acción individual dentro de un job
- Ejemplo: ejecutar un comando, descargar archivos
- Unidad mínima de trabajo

#### Ejemplo simple de estructura:

```yaml
# Pipeline básico con 2 stages
stages:
- stage: Build
  jobs:
  - job: CompileCode
    steps:
    - script: 'echo Compilando...'
    - script: 'dotnet build'

- stage: Deploy  
  jobs:
  - job: DeployApp
    steps:
    - script: 'echo Desplegando...'
    - script: 'deploy.sh'
```

#### Jobs en paralelo vs secuencial:

**Paralelo** (por defecto en un stage):
```yaml
- stage: Tests
  jobs:
  - job: UnitTests
    steps:
    - script: 'run unit tests'
  
  - job: IntegrationTests  # Se ejecuta AL MISMO TIEMPO
    steps:
    - script: 'run integration tests'
```

**Secuencial** (con dependencias):
```yaml
- stage: Deploy
  jobs:
  - job: DeployDatabase
    steps:
    - script: 'deploy database'
  
  - job: DeployApp
    dependsOn: DeployDatabase  # Espera a que termine DeployDatabase
    steps:
    - script: 'deploy application'
```

#### Dependencias y Control de Errores:

**Dependencias entre Stages:**
```yaml
stages:
- stage: Build
  jobs:
  - job: CompileApp
    steps:
    - script: 'dotnet build'

- stage: Test
  dependsOn: Build  # Solo se ejecuta si Build es exitoso
  jobs:
  - job: RunTests
    steps:
    - script: 'dotnet test'

- stage: Deploy
  dependsOn: Test   # Solo se ejecuta si Test es exitoso
  jobs:
  - job: DeployApp
    steps:
    - script: 'deploy.sh'
```

**Continuar aunque falle** (condition):
```yaml
- stage: Deploy
  dependsOn: Test
  condition: always()  # Se ejecuta SIEMPRE, incluso si Test falla
  jobs:
  - job: Cleanup
    steps:
    - script: 'cleanup resources'

- stage: Notify
  dependsOn: Deploy  
  condition: failed()  # Solo se ejecuta si Deploy falló
  jobs:
  - job: SendAlert
    steps:
    - script: 'send error notification'
```

**Múltiples dependencias:**
```yaml
stages:
- stage: BuildFrontend
  jobs:
  - job: BuildUI
    steps: 
    - script: 'npm run build'

- stage: BuildBackend  
  jobs:
  - job: BuildAPI
    steps:
    - script: 'dotnet build'

- stage: Deploy
  dependsOn:  # Espera a que terminen AMBOS
  - BuildFrontend
  - BuildBackend
  jobs:
  - job: DeployAll
    steps:
    - script: 'deploy frontend and backend'
```

#### Conditions más comunes:
- **`succeeded()`**: Solo si el stage anterior fue exitoso (por defecto)
- **`failed()`**: Solo si el stage anterior falló  
- **`always()`**: Siempre se ejecuta, independientemente del resultado
- **`canceled()`**: Solo si el pipeline fue cancelado

### Variables en Azure DevOps

#### ¿Qué son las Variables?
Las **variables** almacenan valores que pueden reutilizarse en todo el pipeline, como connection strings, URLs, nombres de aplicaciones, etc.

#### Tipos de Variables:

**Variables en YAML** (definidas en el pipeline):
```yaml
variables:
- name: 'appName'
  value: 'mi-aplicacion'
- name: 'environment'
  value: 'production'

stages:
- stage: Deploy
  jobs:
  - job: DeployApp
    steps:
    - script: 'echo Desplegando $(appName) en $(environment)'
```

**Variables por Stage**:
```yaml
stages:
- stage: QA
  variables:
  - name: 'environment'
    value: 'qa'
  - name: 'webAppName'  
    value: 'miapp-qa'
  jobs:
  - job: Deploy
    steps:
    - script: 'echo Deploy to $(webAppName)'

- stage: PROD
  variables:
  - name: 'environment'
    value: 'prod'
  - name: 'webAppName'
    value: 'miapp-prod'
```

#### Dónde crear Variables en Azure DevOps:

**1. Variable Groups (Grupos de Variables):**
- **Ubicación**: Library > Variable groups
- **Uso**: Compartir variables entre múltiples pipelines
- **Ejemplo**: Configuraciones de base de datos, URLs de APIs

**Pasos para crear Variable Group:**
1. Ir a Azure DevOps > Pipelines > Library
2. Click "+ Variable group"  
3. Nombre: "QA-Variables"
4. Agregar variables:
   - `dbConnectionString`: "Server=qa-db;Database=myapp"
   - `apiUrl`: "https://api-qa.miapp.com"
5. Save

**Usar Variable Group en YAML:**
```yaml
variables:
- group: 'QA-Variables'  # Importa todas las variables del grupo

stages:
- stage: Deploy
  jobs:
  - job: DeployApp
    steps:
    - script: 'echo API URL: $(apiUrl)'
    - script: 'echo DB: $(dbConnectionString)'
```

**2. Pipeline Variables:**
- **Ubicación**: Pipeline > Edit > Variables tab
- **Uso**: Variables específicas de un pipeline
- **Alcance**: Todo el pipeline

**Pasos para crear Pipeline Variables:**
1. Abrir tu pipeline en Azure DevOps
2. Click "Edit" 
3. Click "Variables" (esquina superior derecha)
4. Click "+ Add"
5. Nombre: "buildConfiguration", Valor: "Release"
6. Save

**3. Environment Variables:**
- **Ubicación**: Pipelines > Environments > [Environment] > Variables
- **Uso**: Variables específicas de un entorno (QA, PROD)
- **Ejemplo**: URLs específicas por entorno

#### Variables Predefinidas del Sistema:
```yaml
steps:
- script: |
    echo "Build ID: $(Build.BuildId)"
    echo "Source Branch: $(Build.SourceBranch)" 
    echo "Repository Name: $(Build.Repository.Name)"
    echo "Agent OS: $(Agent.OS)"
```

#### Variables del Pipeline (UI de Azure DevOps)

**¿Para qué sirven?**
- Almacenar valores que cambian según ejecución (versiones, entornos, configuraciones)
- Permitir override manual cuando se ejecuta el pipeline
- Gestionar información sensible (passwords, tokens)

**Cómo crear Variables del Pipeline:**
1. Abrir tu pipeline en Azure DevOps
2. Click "Edit"
3. Click "Variables" (esquina superior derecha)
4. Click "+ New variable"
5. Configurar:
   - **Name**: nombre de la variable (ej: `webAppName`)
   - **Value**: valor por defecto (ej: `mi-app-qa`)
   - **Keep this value secret**: ☑️ para passwords/tokens
   - **Let users override**: ☑️ para permitir cambios en ejecución

#### Cómo usar Variables en YAML:

**Referencia básica en YAML:**
```yaml
variables:
  # Variables definidas en el YAML
  buildConfig: 'Release'
  
stages:
- stage: Deploy
  jobs:
  - job: DeployApp
    steps:
    - task: AzureWebApp@1
      inputs:
        appName: '$(webAppName)'        # Variable del Pipeline UI
        configuration: '$(buildConfig)' # Variable del YAML
```

**Usar variables en scripts:**

**PowerShell:**
```yaml
- task: PowerShell@2
  inputs:
    script: |
      Write-Host "App: ${env:WEBAPPNAME}"
      Write-Host "Config: ${env:BUILDCONFIG}"
      # Las variables se convierten a MAYÚSCULAS y . → _
```

**Bash:**
```yaml
- task: Bash@3
  inputs:
    script: |
      echo "App: $WEBAPPNAME"
      echo "Config: $BUILDCONFIG"
      # Variables automáticamente disponibles como env vars
```

**Batch/CMD:**
```yaml
- task: CmdLine@2
  inputs:
    script: |
      echo App: %WEBAPPNAME%
      echo Config: %BUILDCONFIG%
```

#### Ejemplo práctico completo:

**Variables del Pipeline (creadas en UI):**
- `webAppNameQA` = "miapp-qa-2025"
- `webAppNameProd` = "miapp-prod-2025"
- `azureSubscription` = "Visual Studio Enterprise"
- `dbPassword` = "••••••••" (marcada como secret)

**Pipeline YAML usando las variables:**
```yaml
trigger:
- main

variables:
  buildConfiguration: 'Release'

stages:
- stage: Build
  jobs:
  - job: BuildJob
    steps:
    - script: 'echo Building with $(buildConfiguration)'
    - task: DotNetCoreCLI@2
      inputs:
        projects: '**/*.csproj'
        arguments: '--configuration $(buildConfiguration)'

- stage: DeployQA
  jobs:
  - deployment: QADeploy
    environment: 'QA'
    strategy:
      runOnce:
        deploy:
          steps:
          - task: AzureWebApp@1
            inputs:
              azureSubscription: '$(azureSubscription)'  # Variable del UI
              appName: '$(webAppNameQA)'                 # Variable del UI
          
          - task: PowerShell@2
            inputs:
              script: |
                Write-Host "Deployed to: ${env:WEBAPPNAMEQA}"
                # Conectar a DB con: ${env:DBPASSWORD}

- stage: DeployProd
  dependsOn: DeployQA
  jobs:
  - deployment: ProdDeploy
    environment: 'Production'
    strategy:
      runOnce:
        deploy:
          steps:
          - task: AzureWebApp@1
            inputs:
              azureSubscription: '$(azureSubscription)'
              appName: '$(webAppNameProd)'              # Variable del UI
```

#### Ventajas de Variables del Pipeline:
- **Override en ejecución**: Cambiar valores sin editar código
- **Reutilización**: Misma variable en múltiples stages
- **Seguridad**: Variables secretas automáticamente ocultas
- **Flexibilidad**: Diferentes valores para diferentes ejecuciones

---

## 3- Consignas a desarrollar:
 - Los despliegues (deployments) de aplicaciones se pueden realizar en diferentes tipos de entornos
   - On-Premise (internos) es decir en servidores propios.
   - Nubes Públicas, ejemplo AWS, Azure, Gcloud, etc.
   - Plataformas como servicios (PaaS), ejemplo Heroku, Google App Engine, AWS, Azure WebApp, etc
 - En esta guía haremos despliegue a Plataforma como Servicio utilizando Azure Web Apps

## 4- Desarrollo:

### 4.1\. Crear cuenta en Azure y configurar recursos

**4.1.1\. Crear cuenta Azure**
- Seguir las instrucciones de la sección 5.1 para crear cuenta gratuita

**4.1.2\. Crear Web App de QA en Azure Portal**
- Crear Resource Group: `rg-tp05-ingsoft3-2025`
- Crear App Service Plan: `plan-tp05-free`
- Crear Web App QA: `webapp-tp05-qa-[apellido]` (reemplazar [apellido] con tu apellido)
- Runtime: `.NET 8 (LTS)`
- OS: Windows
- Publish: Code

**4.1.3\. Crear Web App de PROD**
- Crear Web App PROD: `webapp-tp05-prod-[apellido]`
- Usar el mismo Resource Group y App Service Plan

### 4.2\. Configurar Service Connection en Azure DevOps

**4.2.1\. Crear Service Connection**
- Ir a `https://dev.azure.com/[TUORGANIZACION]/` > Project Settings > Service connections
- Click "New service connection" > "Azure Resource Manager" > "Service principal (automatic)"
- Subscription: Seleccionar tu suscripción Azure
- Resource Group: `rg-tp05-ingsoft3-2025`
- Service connection name: `azure-tp05-connection`
- Grant access permission to all pipelines: ✅

### 4.3\. Crear Pipeline YAML básico (solo CI)

**4.3.1\. Crear archivo `azure-pipelines.yml` en la raíz del repositorio:**

```yaml
# Pipeline CI - Etapa inicial
trigger:
- main

pool:
  vmImage: 'windows-latest'

variables:
  buildConfiguration: 'Release'
  dotNetFramework: 'net8.0'
  dotNetVersion: '8.0.x'

stages:
- stage: Build
  displayName: 'Build and Test'
  jobs:
  - job: BuildJob
    displayName: 'Build and Test Job'
    steps:
    
    - task: UseDotNet@2
      displayName: 'Use .NET 8 SDK'
      inputs:
        packageType: 'sdk'
        version: '$(dotNetVersion)'
    
    - task: DotNetCoreCLI@2
      displayName: 'Restore NuGet packages'
      inputs:
        command: 'restore'
        projects: '**/*.csproj'
    
    - task: DotNetCoreCLI@2
      displayName: 'Build application'
      inputs:
        command: 'build'
        projects: '**/*.csproj'
        arguments: '--configuration $(buildConfiguration) --no-restore'
    
    - task: DotNetCoreCLI@2
      displayName: 'Publish application'
      inputs:
        command: 'publish'
        projects: '**/*.csproj'
        arguments: '--configuration $(buildConfiguration) --output $(Build.ArtifactStagingDirectory) --no-build'
        publishWebProjects: true
        zipAfterPublish: true
    
    - task: PublishBuildArtifacts@1
      displayName: 'Publish build artifacts'
      inputs:
        pathToPublish: '$(Build.ArtifactStagingDirectory)'
        artifactName: 'drop'
        publishLocation: 'Container'
```

**4.3.2\. Crear el pipeline en Azure DevOps**
- Ir a Pipelines > Create Pipeline
- Seleccionar Azure Repos Git
- Seleccionar tu repositorio
- Seleccionar "Existing Azure Pipelines YAML file"
- Seleccionar `/azure-pipelines.yml`
- Click "Run"

### 4.4\. Verificar el build inicial

**4.4.1\. Verificar ejecución del pipeline**
- Verificar que todas las etapas se ejecuten correctamente
- Verificar que se generen los artifacts
- Revisar los logs en caso de errores

### 4.5\. Extender pipeline para incluir deploy a QA

**4.5.1\. Crear Environments en Azure DevOps**
- Ir a Pipelines > Environments
- Crear environment "QA" sin aprobaciones
- Crear environment "PROD" con aprobación manual

**4.5.2\. Actualizar `azure-pipelines.yml` para incluir CD:**

```yaml
# Pipeline CI/CD Completo
trigger:
- main

pool:
  vmImage: 'windows-latest'

variables:
  buildConfiguration: 'Release'
  dotNetFramework: 'net8.0'
  dotNetVersion: '8.0.x'
  azureSubscription: 'azure-tp05-connection'  # Service Connection name

stages:
- stage: Build
  displayName: 'Build and Test'
  jobs:
  - job: BuildJob
    displayName: 'Build and Test Job'
    steps:
    
    - task: UseDotNet@2
      displayName: 'Use .NET 8 SDK'
      inputs:
        packageType: 'sdk'
        version: '$(dotNetVersion)'
    
    - task: DotNetCoreCLI@2
      displayName: 'Restore NuGet packages'
      inputs:
        command: 'restore'
        projects: '**/*.csproj'
    
    - task: DotNetCoreCLI@2
      displayName: 'Build application'
      inputs:
        command: 'build'
        projects: '**/*.csproj'
        arguments: '--configuration $(buildConfiguration) --no-restore'
        
    - task: DotNetCoreCLI@2
      displayName: 'Publish application'
      inputs:
        command: 'publish'
        projects: '**/*.csproj'
        arguments: '--configuration $(buildConfiguration) --output $(Build.ArtifactStagingDirectory) --no-build'
        publishWebProjects: true
        zipAfterPublish: true
    
    - task: PublishBuildArtifacts@1
      displayName: 'Publish build artifacts'
      inputs:
        pathToPublish: '$(Build.ArtifactStagingDirectory)'
        artifactName: 'drop'
        publishLocation: 'Container'

- stage: DeployQA
  displayName: 'Deploy to QA'
  dependsOn: Build
  condition: and(succeeded(), eq(variables['Build.SourceBranch'], 'refs/heads/main'))
  jobs:
  - deployment: DeployQAJob
    displayName: 'Deploy to QA Environment'
    environment: 'QA'
    strategy:
      runOnce:
        deploy:
          steps:
          - task: AzureWebApp@1
            displayName: 'Deploy to QA Web App'
            inputs:
              azureSubscription: '$(azureSubscription)'
              appType: 'webApp'
              appName: 'webapp-tp05-qa-[apellido]'  # Reemplazar con tu apellido
              package: '$(Pipeline.Workspace)/drop/**/*.zip'
              deploymentMethod: 'auto'
```

### 4.6\. Probar el deployment a QA

**4.6.1\. Hacer un cambio en el código**
- Modificar `WeatherForecastController.cs` para devolver 7 pronósticos:

```csharp
[HttpGet(Name = "GetWeatherForecast")]
public IEnumerable<WeatherForecast> Get()
{
    return Enumerable.Range(1, 7).Select(index => new WeatherForecast  // Cambiar de 5 a 7
    {
        Date = DateOnly.FromDateTime(DateTime.Now.AddDays(index)),
        TemperatureC = Random.Shared.Next(-20, 55),
        Summary = Summaries[Random.Shared.Next(Summaries.Length)]
    })
    .ToArray();
}
```

**4.6.2\. Hacer commit y push**
```bash
git add .
git commit -m "Change forecast count to 7 items"
git push origin main
```

**4.6.3\. Verificar deployment**
- Ver ejecución del pipeline en Azure DevOps
- Navegar a `https://webapp-tp05-qa-[apellido].azurewebsites.net/weatherforecast`
- Verificar que devuelve 7 elementos

### 4.7\. Agregar stage de deployment a PROD

**4.7.1\. Actualizar `azure-pipelines.yml` para incluir PROD:**

```yaml
# Pipeline CI/CD Completo con QA y PROD
trigger:
- main

pool:
  vmImage: 'windows-latest'

variables:
  buildConfiguration: 'Release'
  dotNetFramework: 'net8.0'
  dotNetVersion: '8.0.x'
  azureSubscription: 'azure-tp05-connection'

stages:
- stage: Build
  displayName: 'Build and Test'
  jobs:
  - job: BuildJob
    displayName: 'Build and Test Job'
    steps:
    
    - task: UseDotNet@2
      displayName: 'Use .NET 8 SDK'
      inputs:
        packageType: 'sdk'
        version: '$(dotNetVersion)'
    
    - task: DotNetCoreCLI@2
      displayName: 'Restore NuGet packages'
      inputs:
        command: 'restore'
        projects: '**/*.csproj'
    
    - task: DotNetCoreCLI@2
      displayName: 'Build application'
      inputs:
        command: 'build'
        projects: '**/*.csproj'
        arguments: '--configuration $(buildConfiguration) --no-restore'
        
    - task: DotNetCoreCLI@2
      displayName: 'Publish application'
      inputs:
        command: 'publish'
        projects: '**/*.csproj'
        arguments: '--configuration $(buildConfiguration) --output $(Build.ArtifactStagingDirectory) --no-build'
        publishWebProjects: true
        zipAfterPublish: true
    
    - task: PublishBuildArtifacts@1
      displayName: 'Publish build artifacts'
      inputs:
        pathToPublish: '$(Build.ArtifactStagingDirectory)'
        artifactName: 'drop'
        publishLocation: 'Container'

- stage: DeployQA
  displayName: 'Deploy to QA'
  dependsOn: Build
  condition: and(succeeded(), eq(variables['Build.SourceBranch'], 'refs/heads/main'))
  jobs:
  - deployment: DeployQAJob
    displayName: 'Deploy to QA Environment'
    environment: 'QA'
    strategy:
      runOnce:
        deploy:
          steps:
          - task: AzureWebApp@1
            displayName: 'Deploy to QA Web App'
            inputs:
              azureSubscription: '$(azureSubscription)'
              appType: 'webApp'
              appName: 'webapp-tp05-qa-[apellido]'
              package: '$(Pipeline.Workspace)/drop/**/*.zip'
              deploymentMethod: 'auto'

- stage: DeployPROD
  displayName: 'Deploy to PROD'
  dependsOn: DeployQA
  condition: and(succeeded(), eq(variables['Build.SourceBranch'], 'refs/heads/main'))
  jobs:
  - deployment: DeployPRODJob
    displayName: 'Deploy to PROD Environment'
    environment: 'PROD'
    strategy:
      runOnce:
        deploy:
          steps:
          - task: AzureWebApp@1
            displayName: 'Deploy to PROD Web App'
            inputs:
              azureSubscription: '$(azureSubscription)'
              appType: 'webApp'
              appName: 'webapp-tp05-prod-[apellido]'
              package: '$(Pipeline.Workspace)/drop/**/*.zip'
              deploymentMethod: 'auto'
```

### 4.8\. Configurar aprobación manual para PROD

**4.8.1\. Configurar Environment PROD con aprobación**
- Ir a Pipelines > Environments > PROD
- Click en los tres puntos (...) > Approvals and checks
- Click "+" > Approvals
- Añadir usuarios que pueden aprobar (tu usuario)
- Timeout: 30 días
- Minimum approvers: 1
- Save

### 4.9\. Probar el flujo completo con aprobación

**4.9.1\. Cambiar código a 10 pronósticos**
```csharp
return Enumerable.Range(1, 10).Select(index => new WeatherForecast  // Cambiar a 10
```

**4.9.2\. Commit y push**
```bash
git add .
git commit -m "Change forecast count to 10 items"
git push origin main
```

**4.9.3\. Verificar flujo**
- Pipeline ejecuta Build ✅
- Deploy a QA se ejecuta automáticamente ✅
- Deploy a PROD queda pendiente de aprobación ⏳
- Verificar QA: `https://webapp-tp05-qa-[apellido].azurewebsites.net/weatherforecast`
- Verificar PROD: `https://webapp-tp05-prod-[apellido].azurewebsites.net/weatherforecast` (debería tener versión anterior)

### 4.10\. Aprobar deployment a PROD

**4.10.1\. Aprobar desde la interfaz**
- Ir a Pipelines > Environment > PROD > Pending deployments
- Click "Review" > "Approve"
- O aprobar desde el email recibido

**4.10.2\. Opciones de aprobación**
- Se puede aprobar inmediatamente
- Se puede posponer la aprobación hasta una fecha específica
- Se puede rechazar con comentarios

### 4.11\. Verificar deployment final

**4.11.1\. Confirmar deployment exitoso**
- Esperar que termine el stage "Deploy to PROD"
- Verificar PROD: `https://webapp-tp05-prod-[apellido].azurewebsites.net/weatherforecast`
- Confirmar que ahora devuelve 10 elementos
- Verificar que QA y PROD tienen la misma versión

### 4.12\. Probar un segundo ciclo con aprobación pospuesta

**4.12.1\. Cambiar a 5 pronósticos**
```csharp
return Enumerable.Range(1, 5).Select(index => new WeatherForecast  // Cambiar a 5
```

**4.12.2\. Commit, push y verificar**
- QA se actualiza automáticamente con 5 elementos
- PROD queda pendiente de aprobación (mantiene 10 elementos)
- Aprobar deployment después de verificar QA
- Confirmar que PROD se actualiza a 5 elementos


## 5- Instructivos:





#### 5.2 Crear un recurso Web App en Azure Portal

5.2.1\. Navegar to [https://portal.azure.com/#home](https://portal.azure.com/#home)


5.2.2\. Click en este icono.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/699d7c93-5e30-40b0-a784-bb232bbeef4c/ascreenshot.jpeg?tl_px=0,0&br_px=859,480&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=264,115)


5.2.3\. Click en "Buscar servicios y marketplace" .

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/477981f4-6ee2-48c3-9261-97b59fe2b635/ascreenshot.jpeg?tl_px=0,0&br_px=859,480&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=348,125)


5.2.4\. Click en esta imagen.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/a7944fc4-82ba-4c2e-a29e-569b5aa921dd/ascreenshot.jpeg?tl_px=681,119&br_px=1541,600&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=432,212)


5.2.5\. Click "Planes"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/d91bb0c2-afbb-47b9-93eb-f955a67052f4/ascreenshot.jpeg?tl_px=0,151&br_px=859,632&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=176,212)


5.2.6\. Click "Información de uso y soporte técnico"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/79fcfefb-3d2f-4472-b42b-42ff1c5195c7/ascreenshot.jpeg?tl_px=0,147&br_px=859,628&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=296,212)


5.2.7\. Click "Crear"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/a0fb8565-6a6a-4e9e-965a-a611ccc7de29/ascreenshot.jpeg?tl_px=0,48&br_px=859,529&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=389,212)


5.2.8\. Click "(Nuevo) Grupo de recursos"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/2791e4a8-db6d-4896-8868-467108714fb9/ascreenshot.jpeg?tl_px=0,176&br_px=859,657&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=353,212)


5.2.9\. Click "(Nuevo) Grupo de recursos"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/d9b09799-ea72-4540-bbcc-a6f990abbc2d/ascreenshot.jpeg?tl_px=0,168&br_px=859,649&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=350,212)


5.2.10\. Click "Crear nuevo"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/4c2d2cc6-716d-4a43-940e-06155eb3c6fa/ascreenshot.jpeg?tl_px=0,198&br_px=859,679&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=304,212)


5.2.11\. Type "TPSIngSoft3UCC2024"


5.2.12\. Click aquí.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/f3d93ccc-201a-47c4-8036-d2f1447d4885/ascreenshot.jpeg?tl_px=0,371&br_px=859,852&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=342,212)


5.2.13\. Click en "Nombre".

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/74ca7403-ac4c-4401-8b96-6973cdda1f40/ascreenshot.jpeg?tl_px=0,281&br_px=859,762&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=397,212)


5.2.14\. Type "MiWebApp01"


5.2.15\. Click aqui.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/d5687ed0-9f00-4f8f-9f62-ee4c5f8a1338/ascreenshot.jpeg?tl_px=0,327&br_px=859,808&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=273,212)


5.2.16\. Click "Seleccione una pila del entorno en tiempo de ejecución"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/b36e2e59-cf3d-49d1-b9ca-15952ab57e4d/ascreenshot.jpeg?tl_px=0,421&br_px=859,902&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=371,212)


5.2.17\. Click aqui.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/0a8973c1-da85-4a4f-bfff-013d94d056a3/ascreenshot.jpeg?tl_px=0,0&br_px=859,480&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=352,187)


5.2.18\. Click "Estándar S1 (Total de ACU: 100, 1.75 GB de memoria, 1 vCPU)"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/5072c787-029a-43dc-a5c7-abfae4e86271/ascreenshot.jpeg?tl_px=0,448&br_px=859,929&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=374,212)


5.2.19\. Click "60 minutos de CPU/día incluidos"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/e74b0a6b-6e32-4dbe-b801-24dcd7612c75/ascreenshot.jpeg?tl_px=3,79&br_px=862,560&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


5.2.20\. Click "Crear nuevo"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/abd461a8-c319-4a32-80ba-4560a296f5f0/ascreenshot.jpeg?tl_px=0,338&br_px=859,819&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=303,212)


5.2.21\. Type "MiAppPlan01"


5.2.22\. Click "Aceptar"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/2ce05660-43b4-403a-84ea-a49738fc066a/ascreenshot.jpeg?tl_px=0,476&br_px=859,957&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=328,212)


5.2.23\. Click "Siguiente: Base de datos >"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/3e6e97a1-2cae-4aa2-b3db-3d7b7aac6d54/ascreenshot.jpeg?tl_px=0,487&br_px=859,968&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=323,411)


5.2.24\. Click "Siguiente: Implementación >"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/4440896d-cc16-4e3d-937e-ced647c789a0/ascreenshot.jpeg?tl_px=0,487&br_px=859,968&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=339,421)


5.2.25\. Click "Deshabilitar"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/fc5e4758-8230-46dd-a4c0-9b0fc1bfe82f/ascreenshot.jpeg?tl_px=0,47&br_px=859,528&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=295,212)


5.2.26\. Click "Siguiente: Redes >"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/ed6fd2e0-5c15-484b-99cd-3509e5c359aa/ascreenshot.jpeg?tl_px=0,487&br_px=859,968&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=345,423)


5.2.27\. Click "Siguiente: Supervisión y protección >"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/6a9471d7-b0fc-46d0-92de-8618bff28d05/ascreenshot.jpeg?tl_px=0,487&br_px=859,968&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=389,420)


5.2.28\. Click aquí.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/51e65a51-2a51-44d9-acd2-ddde9c4cb4b4/ascreenshot.jpeg?tl_px=0,176&br_px=859,657&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=265,212)


5.2.29\. Click "Siguiente: Etiquetas >"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/2008e818-fbb7-4b9d-b0ba-9bd2f25963e2/ascreenshot.jpeg?tl_px=0,487&br_px=859,968&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=319,416)


5.2.30\. Click "Siguiente: Revisar y crear >"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/b36b996a-0746-42c6-b8f5-900a98fec43b/ascreenshot.jpeg?tl_px=0,487&br_px=859,968&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=349,424)


5.2.31\. Click "Descargar una plantilla para la automatización"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/23e9fc9f-f718-41a7-96c0-bc9a74c4d469/ascreenshot.jpeg?tl_px=29,487&br_px=888,968&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,423)


5.2.32\. Click here.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/e53fc21f-5705-4e03-8e7a-1d94e6d92222/ascreenshot.jpeg?tl_px=0,0&br_px=859,480&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=74,122)


5.2.33\. Click "Crear"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/9dccaabe-c69a-489b-89aa-5a77c551cf7f/ascreenshot.jpeg?tl_px=0,487&br_px=859,968&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=42,419)


5.2.34\. Click this icon.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/aaf4fc77-d72b-4d8a-bffb-86bfd549890d/ascreenshot.jpeg?tl_px=681,0&br_px=1541,480&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=484,-7)


5.2.35\. Click "Ir al recurso"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/a2151089-bcdd-454e-972c-0848d573ddb0/ascreenshot.jpeg?tl_px=0,241&br_px=859,722&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=378,212)


5.2.36\. Click this icon.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/5077baf7-20b0-4f24-8c45-80806b044ccf/ascreenshot.jpeg?tl_px=681,0&br_px=1541,480&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=791,34)


5.2.37\. Click "[miwebapp01.azurewebsites.net](http://miwebapp01.azurewebsites.net)"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/a250a47b-4fa8-4a28-a200-dce85d724a23/ascreenshot.jpeg?tl_px=681,17&br_px=1541,498&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=431,212)


5.2.38\. Click here.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/57cc8aa4-cde7-444c-8c40-b9f15a32e76c/ascreenshot.jpeg?


#### 5.3 Clonar una Web App a partir de un Template Deployment en Azure Portal

5.3.1\. Navigate to [https://portal.azure.com/#home](https://portal.azure.com/#home)


5.3.2\. Click en el Grupo de Recursos

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/6264b362-a4c8-4b2b-84b6-8db640b42c74/ascreenshot.jpeg?tl_px=0,0&br_px=2182,1664&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=178,454)


5.3.3\. Crear un nuevo recurso

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/b6873b5f-643b-4333-a7e8-082034aa178b/ascreenshot.jpeg?tl_px=0,0&br_px=1719,961&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=400,169)


5.3.4\. Buscar el recurso "Template Deployment (implementar mediante plantillas personalizadas)" y seleccionarlo

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/1c2f28d6-9d90-4eae-8787-8e2c2d5c6674/ascreenshot.jpeg?tl_px=0,565&br_px=1719,1526&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=371,276)


5.3.5\. Click "Crear"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/14e4d429-e482-42bb-9ed2-8767fd40c1fe/ascreenshot.jpeg?tl_px=19,127&br_px=1739,1088&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=524,277)


5.3.6\. Click "Cree su propia plantilla en el editor."

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/27bdd534-57f4-4174-b3df-783e0a7a1746/ascreenshot.jpeg?tl_px=0,59&br_px=1719,1020&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=300,277)


5.3.7\. Descomprimir el archivo template.zip que se descargó en los pasos 31 y 32 del instructivo "**Crear una Web App en Azure Portal"**

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/9c9eebbf-f5d5-44f2-a49e-0661b5060e8a/screenshot.jpeg?tl_px=0,0&br_px=1522,996&force_format=jpeg&q=100&width=1120.0)


5.3.8\. Cargamos el archivo template.json que se extrajo del archivo template.zip que se descargó en los pasos 31 y 32 del instructivo "**Crear una Web App en Azure Portal"**

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/0ef39e7d-5e48-44be-9bdb-4c06e96d7e0a/ascreenshot.jpeg?tl_px=0,0&br_px=1719,961&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=461,156)


5.3.9\. Click "Guardar"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/a9263d36-b7d1-46fb-b299-2e8eabf79e30/ascreenshot.jpeg?tl_px=0,0&br_px=2182,1664&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=40,791)


5.3.10\. Click "Editar parámetros"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/51078d96-f220-416c-860f-969f4e91969c/ascreenshot.jpeg?tl_px=0,0&br_px=2182,1281&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=502,286)


5.3.11\. Cargamos el archivo parameters.json que se extrajo del archivo template.zip que se descargó en los pasos 31 y 32 del instructivo "**Crear una Web App en Azure Portal"**

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/4504a6c3-50c6-4746-8615-301e5461f840/ascreenshot.jpeg?tl_px=0,0&br_px=1719,961&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=107,165)


5.3.12\. Click "Guardar"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/7c5f7826-e42e-4b3a-b08e-6dc3ab5bd16c/ascreenshot.jpeg?tl_px=0,702&br_px=1719,1664&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=43,550)


5.3.13\. Click "Editar parámetros"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/4a68dfcf-97e1-458f-9581-ae1f5e5488a3/ascreenshot.jpeg?tl_px=168,167&br_px=1887,1128&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=524,277)


5.3.14\. Cambiamos el nombre de nuestra Web App agregandole el sufijo "-PROD"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/17b563b6-bb8b-4d12-92df-7db192f5039f/ascreenshot.jpeg?tl_px=0,293&br_px=1719,1254&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=315,277)


5.3.15\. Type "-PROD"


5.3.16\. Click em Guardar

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/4aa5598f-03e1-4ef9-82ac-57d74b060c6f/user_cropped_screenshot.jpeg?tl_px=0,382&br_px=2182,1664&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=14,603)


5.3.17\. Click "Siguiente"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/834d9d7b-89fe-436e-8d6d-1dba404a0c1c/ascreenshot.jpeg?tl_px=0,0&br_px=2182,1664&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=160,782)


5.3.18\. Click "Crear"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/88709564-ee13-449c-9603-7e2c88ab219d/ascreenshot.jpeg?tl_px=0,0&br_px=2182,1664&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=257,787)


5.3.19\. Click "Ir al grupo de recursos"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/289c3cc8-4754-4ec0-b5e3-c7bdc65648af/ascreenshot.jpeg?tl_px=0,0&br_px=2182,1664&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=380,446)


5.3.20\. Actualizamos el Grupo de Recursos

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/61ee0cc7-7936-4a4f-bae5-91a842c35817/ascreenshot.jpeg?tl_px=0,0&br_px=2182,1281&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=713,113)


5.3.21\. Vemos que hemos clonado la WebApp correctamente:

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/44d1178a-26c3-43aa-b48e-5f2bc79b7c5a/user_cropped_screenshot.jpeg?tl_px=314,25&br_px=2607,1306&force_format=jpeg&q=100&width=1120.0)


-

# Trabajo Práctico 05 – Azure DevOps Release Pipelines (2025)

## 🎯 Objetivo

Implementar un **Release Pipeline completo** que automatice el despliegue de una aplicación desde **QA hasta Producción** utilizando **servicios cloud de tu elección** (Azure Web Apps, AWS EC2/ECS, Google Cloud Run, etc.), con **aprobaciones manuales** y **estrategias de rollback**.

Este trabajo se aprueba **solo si podés explicar qué hiciste, por qué lo hiciste y cómo lo resolviste**.

---

## 🧩 Escenario (actualizado)

Como líder técnico, debés:
1. Tomar la aplicación del **TP04** (o crear una nueva) con **Front + Back + DB**.  
2. Configurar **servicios cloud** para entornos de **QA** y **Producción** (Azure Web Apps, AWS EC2/ECS, Google Cloud Run, Heroku, etc.).  
3. Crear un **Release Pipeline** (Azure DevOps, GitHub Actions, AWS CodePipeline, etc.) que tome artefactos del Build Pipeline y los despliegue automáticamente.  
4. Implementar **aprobaciones manuales** para el pase a Producción.  
5. El despliegue debe incluir:  
   - Configuración de **variables por entorno** (connection strings, URLs, etc.).  
   - **Health checks** post-despliegue.  


---

## 📋 Tareas que debés cumplir

### 1. Configuración de Cloud Resources
- Crear **servicios cloud** para QA y Producción (Azure Web Apps, AWS EC2/ECS, Google Cloud Run, etc.).  
- Configurar variables de entorno.  
- Documentar recursos creados y su propósito.

### 2. Release Pipeline Configuration
- Configurar **Release Pipeline** (Azure DevOps, GitHub Actions, AWS CodePipeline, etc.) conectado al Build Pipeline del TP04.  
- Definir **stages** para QA y Producción con diferentes configuraciones.  


### 3. Gestión de aprobaciones y gates
- Configurar **aprobaciones manuales** para el pase a Producción.    
- Documentar proceso de aprobación y responsables.


### 4. Evidencias y documentación
- Capturas de configuración de servicios cloud, releases exitosos, health checks.  
- Documentar en `decisiones.md` las decisiones técnicas tomadas.

---

## 🔧 Pasos sugeridos (checklist)

1. **Cloud Resources**
   - Crear recursos cloud para QA + PROD (Azure, AWS, GCP, etc.).  
2. **Release Pipeline**
   - Conectar con Build Pipeline, configurar stages QA/PROD.  
3. **Variables y Secrets**
   - Configurar variables y secretos por entorno.  
4. **Aprobaciones**
   - Implementar aprobación manual QA → PROD.  
5. **Health Checks**
   - Validar despliegues con endpoints de salud.   
7. **Evidencias**
   - Capturas y explicación en `decisiones.md`.

---

## 📄 Entregables

1. **Acceso al proyecto de CI/CD** con:
   - **Pipeline** configurado con stages Build, QA y Producción.  
   - Ejecuciones exitosas con aprobaciones manuales funcionando.  


2. **Recursos Cloud** configurados:
   - **Servicios cloud** funcionando en QA y Producción.  
   - Variables  configuradas correctamente.

3. **Repositorio en GitHub** actualizado con:
   - **README.md**: cómo acceder a los servicios, URLs de QA y PROD, proceso de despliegue.  
   - **decisiones.md** con:  
     - Arquitectura de release elegida (herramientas y servicios cloud utilizados).  
     - Configuración de entornos y variables.  
     - Estrategia de aprobaciones implementada.  
     - Evidencias (capturas) de releases exitosos.

4. **URL del proyecto** en la planilla:  
   - [Planilla de TPs](https://docs.google.com/spreadsheets/d/1mZKJ8FH390QHjwkABokh3Ys6kMOFZGzZJ3-kg5ziELc/edit?gid=0#gid=0)

---

## 🗣️ Defensa Oral Obligatoria

Preguntas típicas:
- ¿Por qué elegiste esta herramienta de CI/CD para este escenario?  
- ¿Cómo gestionás variables sensibles entre entornos?  
- ¿Qué criterios usás para aprobar un pase a Producción?  
- ¿Cómo validás que un despliegue fue exitoso?  
- ¿Cómo ejecutás un rollback y en qué situaciones?

---

## ✅ Evaluación

| Criterio                                                    | Peso |
|-------------------------------------------------------------|------|
| Release Pipeline funcionando (QA + PROD)                   | 25%  |
| Configuración correcta de servicios cloud y variables      | 10%  |
| Aprobaciones manuales y gestión de entornos                | 15%  |
| Defensa oral: comprensión y argumentación                  | 50%  |

---

## ⚠️ Uso de IA

Podés usar IA (ChatGPT, Copilot), pero **deberás declarar qué parte fue generada con IA** y justificar cómo la verificaste.  
Si no podés defenderlo, **no se aprueba**.

---

## 📎 Anexo: Documentación y Recursos Adicionales

- https://learn.microsoft.com/en-us/azure/devops/?view=azure-devops
- https://learn.microsoft.com/en-us/azure/devops/pipelines/?view=azure-devops
- https://learn.microsoft.com/en-us/azure/devops/pipelines/yaml-schema/?view=azure-pipelines
- https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/overview
- https://learn.microsoft.com/en-us/azure/azure-resource-manager/templates/overview


