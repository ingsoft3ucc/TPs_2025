# ⚠️ IMPORTANTE – Guía de Práctica Sugerida

Lo que vas a ver a continuación es una **guía paso a paso altamente sugerida** para que practiques el uso de contenedores en Azure, Azure Container Registry, Azure Container Instances y Azure App Services.
**Te recomendamos hacerla completa**, ya que te ayudará a adquirir los conocimientos necesarios.

---

## PERO: Esta guía **NO es el trabajo práctico** que tenés que entregar

El trabajo práctico será evaluado en base a:
- Tu capacidad para **implementar y configurar servicios de contenedores con criterio técnico**.
- Tu capacidad para **explicar y justificar cada decisión que tomaste** sobre qué servicio usar y por qué.
- Una **defensa oral obligatoria** donde vas a tener que demostrar lo que sabés.

---

## ¿Dónde está el trabajo práctico?

El **TP real que debés entregar y defender** se encuentra al final de este archivo.
No alcanza con copiar esta guía. **Si no podés defenderlo, no se aprueba.**

---

## Sobre esta guía

- Esta guía NO es exhaustiva.
- Los servicios de contenedores en Azure requieren **investigación y práctica fuera de clase**.
- En 2 horas no vas a aprender todo sobre estos servicios. **Esto es solo el punto de partida.**
- **IMPORTANTE**: Esta guía práctica está basada en la **aplicación Angular + .NET Core del TP04**. Es solo para practicar conceptos.

---

## 💰 MUY IMPORTANTE - ADVERTENCIA DE COSTOS

**⚠️ Los siguientes servicios de Azure utilizados en esta guía TIENEN COSTO:**

### Servicios con Costo en Azure:

1. **Azure Container Registry (ACR)** 💸
   - Plan Básico: ~$5 USD/mes
   - Plan Standard: ~$20 USD/mes
   - **Costo adicional**: Almacenamiento y transferencia de datos

2. **Azure Container Instances (ACI)** 💸
   - Facturación por segundo de uso
   - Aproximadamente: $0.0000125/segundo por vCPU + $0.0000014/segundo por GB de memoria
   - Ejemplo: 1 vCPU + 1.5GB memoria corriendo 24/7 = ~$40-50 USD/mes

3. **Azure App Services con Contenedores** 💸
   - Requiere App Service Plan (Linux)
   - Plan Basic B1: ~$13 USD/mes (mínimo para producción)
   - Plan Standard S1: ~$70 USD/mes
   - Plan Premium: desde $150 USD/mes

4. **Azure SQL Database** 💸 (si se usa)
   - Plan Básico: desde $5 USD/mes
   - Plan Standard: desde $15 USD/mes

5. **Transferencia de Datos** 💸
   - Salida de datos desde Azure: primeros 100GB gratis/mes, luego se cobra

### Estimación de costo mensual para seguir esta guía completa:
**Aproximadamente $80-150 USD/mes** si dejás los recursos corriendo 24/7.

### Servicios GRATUITOS de Azure (dentro de límites):

1. **Azure DevOps**:
   - ✅ GRATIS para hasta 5 usuarios
   - ✅ 1800 minutos/mes de pipeline gratis
   - ✅ CI/CD pipelines ilimitados

2. **Azure Free Tier** (primer año o créditos para estudiantes):
   - Algunos servicios tienen niveles gratuitos limitados
   - Verificar en: https://azure.microsoft.com/free/

---

## 🎓 Alternativas GRATUITAS para el Trabajo Práctico

**NO estás obligado a usar Azure para el TP**. Podés usar alternativas completamente gratuitas:

### 1. Registro de Contenedores (alternativas a ACR):
- **Docker Hub** (gratis): 1 registro privado + ilimitados públicos
- **GitHub Container Registry** (gratis): Registros privados ilimitados
- **GitLab Container Registry** (gratis): Registros privados ilimitados
- **Quay.io** (gratis): Repositorios públicos ilimitados

### 2. Hosting de Contenedores (alternativas a ACI/App Services):
- **Render.com** (gratis): Deploy de contenedores Docker gratis
- **Railway.app** (gratis): $5 crédito mensual gratis, deploy de contenedores
- **Fly.io** (gratis): 3 VMs pequeñas + 3GB storage gratis
- **Google Cloud Run** (gratis): 2 millones de requests/mes + 180K vCPU-segundos
- **AWS App Runner** (gratis): Capa gratuita limitada
- **Heroku** (gratis con limitaciones): Deploy de contenedores

### 3. CI/CD (alternativas a Azure DevOps):
- **GitHub Actions** (gratis): 2000 minutos/mes para repos públicos, ilimitado para públicos
- **GitLab CI/CD** (gratis): 400 minutos/mes en plan gratuito
- **CircleCI** (gratis): 6000 minutos/mes
- **Travis CI** (gratis para open source)

### 4. Bases de Datos Gratuitas:
- **PostgreSQL/MySQL en Railway.app** (gratis)
- **MongoDB Atlas** (gratis): 512MB
- **PlanetScale** (gratis): 5GB MySQL
- **Supabase** (gratis): PostgreSQL con 500MB
- **CockroachDB** (gratis): 5GB serverless

### 5. Orquestación de Contenedores (si necesitás algo más complejo):
- **Kubernetes local**: Minikube, Kind, k3s (gratis, local)
- **Google Kubernetes Engine (GKE)**: Free tier limitado
- **Civo Cloud**: $250 crédito de prueba

---

## ⚡ Recomendaciones para Evitar Costos en la Guía

Si querés practicar con Azure sin gastar:

1. **Usar Azure for Students**: $100 crédito gratis (renovable anualmente)
   - https://azure.microsoft.com/free/students/

2. **Eliminar recursos inmediatamente después de usarlos**:
   ```bash
   # Eliminar grupo de recursos completo
   az group delete --name NOMBRE_GRUPO --yes
   ```

3. **Usar el tier GRATIS de Azure DevOps** (suficiente para el TP)

4. **Configurar alertas de facturación** en Azure Portal
   - Budget: $10 USD/mes
   - Alerta al 80% de uso

5. **Apagar/eliminar contenedores cuando no los uses**:
   ```bash
   # Detener ACI
   az container stop --name NOMBRE --resource-group GRUPO

   # Eliminar ACI
   az container delete --name NOMBRE --resource-group GRUPO
   ```

---

# Guía Paso a Paso – Contenedores en Azure y Automatización (Práctica sugerida)

## 1- Objetivos de Aprendizaje

Al finalizar esta sesión, los estudiantes serán capaces de:

1. **Seleccionar el servicio de contenedores más adecuado para diferentes escenarios de despliegue en la nube.**
2. **Configurar y utilizar Azure Container Registry (ACR) para almacenar imágenes Docker de manera segura.**
3. **Automatizar la creación y gestión de recursos en Azure mediante scripts y comandos de Azure CLI.**
4. **Utilizar variables y secretos de manera eficiente y segura en los pipelines de Azure DevOps.**
5. **Desarrollar y ejecutar un pipeline CI/CD completo que incluya la construcción y despliegue de contenedores en Azure.**

## 2- Unidad temática que incluye este trabajo práctico
Este trabajo práctico corresponde a la unidad Nº: 2 (Libro Ingeniería de Software: Unidad 18)

## 3- Algunos conceptos fundamentales

### Servicios de Contenedores en Azure

Azure ofrece una amplia variedad de servicios para desplegar y gestionar aplicaciones basadas en contenedores, cada uno adecuado para diferentes escenarios y niveles de complejidad. A continuación se detallan los principales servicios de contenedores que ofrece Azure:

---

#### Azure Container Instances (ACI)
**Despliegue Rápido y Sencillo de Contenedores**

Azure Container Instances es la opción más sencilla y rápida para desplegar contenedores en Azure. ACI permite ejecutar contenedores sin necesidad de gestionar servidores o infraestructuras complejas, lo que lo convierte en una excelente opción para tareas puntuales, desarrollo y pruebas. Además, ACI ofrece facturación por segundo, lo que lo hace muy rentable para cargas de trabajo de corta duración.

**Caso de uso:**
Es ideal para procesamiento por lotes, cargas de trabajo eventuales o aplicaciones que no requieren orquestación avanzada ni alta disponibilidad.

---

#### Azure App Services con Soporte para Contenedores
**Plataforma Gestionada para Aplicaciones Web y APIs en Contenedores**

Azure App Services es una plataforma gestionada para aplicaciones web y APIs que soporta contenedores Docker. Este servicio permite desplegar aplicaciones con alta disponibilidad sin necesidad de gestionar la infraestructura subyacente. Además, App Services ofrece escalabilidad automática, integración con pipelines de CI/CD y soporte para múltiples lenguajes de programación.

**Caso de uso:**
Es ideal para aplicaciones web y APIs que requieren integración con otros servicios de Azure y que necesitan escalabilidad y alta disponibilidad sin preocuparse por la gestión de servidores.

---

#### Azure Kubernetes Service (AKS)
**Orquestación Completa de Contenedores**

Azure Kubernetes Service es la opción gestionada de Kubernetes en Azure, diseñada para proyectos que requieren una orquestación completa de contenedores. AKS permite gestionar clústeres de Kubernetes de manera simplificada, proporcionando escalabilidad automática, distribución de carga y recuperación ante fallos. A pesar de que Azure gestiona la infraestructura subyacente, los usuarios tienen control total sobre el clúster y las configuraciones de Kubernetes.

**Caso de uso:**
AKS es ideal para aplicaciones a gran escala, microservicios y cargas de trabajo críticas que necesitan una orquestación avanzada, alta disponibilidad y flexibilidad en la gestión de contenedores.

---

#### Azure Container Apps
**Orquestación Simplificada con Kubernetes**

Azure Container Apps es una plataforma basada en Kubernetes, pero que abstrae la complejidad de gestionar el clúster. Azure Container Apps permite desplegar y escalar automáticamente aplicaciones basadas en contenedores, ofreciendo escalabilidad basada en eventos y microservicios. Además, se integra con KEDA para escalado automático basado en eventos y con Dapr para facilitar patrones de microservicios.

**Caso de uso:**
Es ideal para microservicios, aplicaciones sin servidor (serverless) y despliegues basados en eventos que requieren escalabilidad, pero sin la necesidad de gestionar Kubernetes directamente.

---

#### Azure Container Registry (ACR)
**Servicio de Azure para Almacenamiento y Gestión de Imágenes de Contenedores**

Azure Container Registry es un servicio seguro y escalable que permite almacenar y gestionar imágenes de contenedores. ACR está diseñado para integrarse fácilmente con otros servicios de Azure, como AKS, ACI, y App Services, proporcionando una solución privada para almacenar y distribuir imágenes Docker. ACR también soporta características avanzadas como la replicación geográfica y políticas de seguridad robustas.

**No es Obligatorio Usar ACR**

Aunque ACR es una opción poderosa y bien integrada en el ecosistema de Azure, no es obligatorio. Es posible desplegar contenedores en Azure utilizando otros registros como Docker Hub o registros privados de terceros. Sin embargo, ACR ofrece mayor seguridad y control en entornos de producción que requieren un manejo más estricto de las imágenes de contenedores.

**Caso de uso:**
ACR es ideal para organizaciones que buscan una solución privada y segura para gestionar imágenes de contenedores dentro de sus entornos de Azure.

---

#### Azure Command Line Interface (Azure CLI)
**Interfaz de Línea de Comandos Unificada**

Azure CLI es una interfaz de línea de comandos que permite interactuar con todos los servicios de Azure. Esta herramienta está diseñada para ser utilizada en scripts y pipelines de CI/CD, facilitando la automatización de tareas como la creación, actualización y eliminación de recursos en Azure. Azure CLI es compatible con Windows, macOS y Linux, y también está disponible en el portal de Azure a través de **Azure Cloud Shell**.

**Ejemplo:**
Con comandos simples como `az group create` o `az container create`, es posible gestionar recursos de Azure como grupos de recursos y contenedores desde cualquier entorno de línea de comandos.

**Caso de uso:**
Azure CLI es ideal para desarrolladores y equipos DevOps que buscan automatizar tareas en Azure de manera rápida y eficiente.

---

#### Variables en Pipelines de Azure DevOps
**Parametrización y Reutilización en Pipelines**

Las variables en los pipelines de Azure DevOps permiten parametrizar y reutilizar información a lo largo del pipeline. Esto facilita la configuración de diferentes entornos (como QA, Staging y Producción) sin necesidad de cambiar el código fuente. Las variables pueden definirse directamente en el archivo YAML del pipeline o en la interfaz de usuario de Azure DevOps.

**Secretos y Seguridad:**

Para manejar información sensible como contraseñas o claves API, se pueden definir variables como secretas, lo que garantiza que no sean expuestas en los logs. También es posible integrar **Azure Key Vault** para gestionar secretos de manera más segura.

**Caso de uso:**
Las variables son esenciales para construir pipelines flexibles y adaptables que puedan desplegarse en múltiples entornos, mientras que los secretos garantizan que la información confidencial esté protegida durante todo el proceso de despliegue.

---

## 4- Desarrollo de la práctica sugerida

### Prerequisitos:
 - Azure CLI instalado

### 4.1 Modificar nuestro pipeline para construir imágenes Docker de back y front y subirlas a ACR

#### 4.1.1 Crear archivos DockerFile para nuestros proyectos de Back y Front

En la raiz de nuestro repo crear una carpeta docker con dos subcarpetas api y front, dentro de cada una de ellas colocar los dockerfiles correspondientes para la creación de imágenes docker en función de la salida de nuestra etapa de Build y Test

![image](https://github.com/user-attachments/assets/2debb92f-9d69-47f1-a85b-72abbc381035)

**DockerFile Back:**
```dockerfile
# Imagen base para ejecutar la aplicación
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS runtime
WORKDIR /app

# Copiar los binarios preconstruidos de la API al contenedor
# Espera que los binarios estén en la carpeta "api" en el contexto de construcción
COPY ./api/ ./

# Exponer los puertos 80 (HTTP) y 443 (HTTPS)
EXPOSE 80
EXPOSE 443

# Configurar las URLs para que la aplicación escuche en HTTP y HTTPS
ENV ASPNETCORE_URLS="http://+:80"
#;https://+:443"

# Comando para ejecutar la aplicación
ENTRYPOINT ["dotnet", "EmployeeCrudApi.dll"]
#CMD ["/bin/bash"]
```

**DockerFile Front:**
```dockerfile
# Utilizar la imagen base de nginx
FROM nginx:alpine

# Establecer el directorio de trabajo en el contenedor
WORKDIR /usr/share/nginx/html

# Eliminar los archivos existentes de la imagen base de nginx
RUN rm -rf ./*

# Copiar los archivos compilados de Angular al directorio de nginx
COPY ./ .

# Exponer el puerto 80 para servir la aplicación Angular
EXPOSE 80
CMD sh -c 'echo "window[\"env\"] = { apiUrl: \"'$API_URL'\" };" > /usr/share/nginx/html/assets/env.js && nginx -g "daemon off;"'
```

#### 4.1.2 Crear un recurso ACR en Azure Portal siguiendo el instructivo 5.1

#### 4.1.3 Modificar nuestro pipeline en la etapa de Build y Test

Luego de la tarea de publicación de los artefactos de Back agregar la tarea de publicación de nuestro dockerfile de back para que esté disponible en etapas posteriores:

```yaml
- task: PublishPipelineArtifact@1
  displayName: 'Publicar Dockerfile de Back'
  inputs:
    targetPath: '$(Build.SourcesDirectory)/docker/api/dockerfile'
    artifact: 'dockerfile-back'
```

Luego de la tarea de publicación de los artefactos de Front agregar la tarea de publicación de nuestro dockerfile de front para que esté disponible en etapas posteriores:

```yaml
- task: PublishPipelineArtifact@1
  displayName: 'Publicar Dockerfile de Front'
  inputs:
    targetPath: '$(Build.SourcesDirectory)/docker/front/dockerfile'
    artifact: 'dockerfile-front'
```

#### 4.1.4 En caso de no contar en nuestro proyecto con una ServiceConnection a Azure Portal para el manejo de recursos, agregar una service connection a Azure Resource Manager como se indica en instructivo 5.2

#### 4.1.5 Agregar a nuestro pipeline variables

```yaml
trigger:
- main

pool:
  vmImage: 'windows-latest'

# AZURE VARIABLES
variables:
  ConnectedServiceName: 'NOMBRE_SERVICE_CONNECTION_AZURE_RESOURCE_MANAGER' #Por ejemplo 'ServiceConnectionARM'
  acrLoginServer: 'URL_DE_RECURSO_ACR' #Por ejemplo 'ascontainerregistry.azurecr.io'
  backImageName: 'NOMBRE_IMAGEN_QA' #Por ejemplo 'employee-crud-api'
```

#### 4.1.6 Agregar a nuestro pipeline una nueva etapa que dependa de nuestra etapa de Build y Test

Agregar tareas para generar imagen Docker de Back:

```yaml
# #----------------------------------------------------------
# ### STAGE BUILD DOCKER IMAGES Y PUSH A AZURE CONTAINER REGISTRY
# #----------------------------------------------------------

- stage: DockerBuildAndPush
  displayName: 'Construir y Subir Imágenes Docker a ACR'
  dependsOn: BuildAndTestBackAndFront #NOMBRE DE NUESTRA ETAPA DE BUILD Y TEST
  jobs:
    - job: docker_build_and_push
      displayName: 'Construir y Subir Imágenes Docker a ACR'
      pool:
        vmImage: 'ubuntu-latest'

      steps:
        - checkout: self

        #----------------------------------------------------------
        # BUILD DOCKER BACK IMAGE Y PUSH A AZURE CONTAINER REGISTRY
        #----------------------------------------------------------

        - task: DownloadPipelineArtifact@2
          displayName: 'Descargar Artefactos de Back'
          inputs:
            buildType: 'current'
            artifactName: 'drop-back'
            targetPath: '$(Pipeline.Workspace)/drop-back'

        - task: DownloadPipelineArtifact@2
          displayName: 'Descargar Dockerfile de Back'
          inputs:
            buildType: 'current'
            artifactName: 'dockerfile-back'
            targetPath: '$(Pipeline.Workspace)/dockerfile-back'

        - task: AzureCLI@2
          displayName: 'Iniciar Sesión en Azure Container Registry (ACR)'
          inputs:
            azureSubscription: '$(ConnectedServiceName)'
            scriptType: bash
            scriptLocation: inlineScript
            inlineScript: |
              az acr login --name $(acrLoginServer)

        - task: Docker@2
          displayName: 'Construir Imagen Docker para Back'
          inputs:
            command: build
            repository: $(acrLoginServer)/$(backImageName)
            dockerfile: $(Pipeline.Workspace)/dockerfile-back/dockerfile
            buildContext: $(Pipeline.Workspace)/drop-back
            tags: 'latest'

        - task: Docker@2
          displayName: 'Subir Imagen Docker de Back a ACR'
          inputs:
            command: push
            repository: $(acrLoginServer)/$(backImageName)
            tags: 'latest'
```

#### 4.1.7 Ejecutar el pipeline y en Azure Portal acceder a la opción Repositorios de nuestro recurso Azure Container Registry. Verificar que exista una imagen con el nombre especificado en la variable backImageName asignada en nuestro pipeline

![image](https://github.com/user-attachments/assets/57f0f0d2-2a23-4a8a-a1a0-6f5d4ea48756)

#### 4.1.8 Agregar tareas para generar imagen Docker de Front (DESAFIO)

A la etapa creada en 4.1.6 Agregar tareas para generar imagen Docker de Front

### 4.2 Desplegar Imágenes en Azure Container Instances (ACI)

#### 4.2.1 Agregar a nuestro pipeline una nueva etapa que dependa de nuestra etapa de Construcción de Imagenes Docker y subida a ACR

Agregar variables a nuestro pipeline:

```yaml
ResourceGroupName: 'NOMBRE_GRUPO_RECURSOS' #Por ejemplo 'TPS_INGSOFT3_UCC'
backContainerInstanceNameQA: 'NOMBRE_CONTAINER_BACK_QA' #Por ejemplo 'as-crud-api-qa'
backImageTag: 'latest'
container-cpu-api-qa: 1 #CPUS de nuestro container de QA
container-memory-api-qa: 1.5 #RAM de nuestro container de QA
```

Agregar variable secreta cnn-string-qa desde la GUI de ADO que apunte a nuestra BD de SQL Server de QA como se indica en el instructivo 5.3

Agregar tareas para crear un recurso Azure Container Instances que levante un contenedor con nuestra imagen de back:

```yaml
#----------------------------------------------------------
### STAGE DEPLOY TO ACI QA
#----------------------------------------------------------

- stage: DeployToACIQA
  displayName: 'Desplegar en Azure Container Instances (ACI) QA'
  dependsOn: DockerBuildAndPush
  jobs:
    - job: deploy_to_aci_qa
      displayName: 'Desplegar en Azure Container Instances (ACI) QA'
      pool:
        vmImage: 'ubuntu-latest'

      steps:
        #------------------------------------------------------
        # DEPLOY DOCKER BACK IMAGE A AZURE CONTAINER INSTANCES QA
        #------------------------------------------------------

        - task: AzureCLI@2
          displayName: 'Desplegar Imagen Docker de Back en ACI QA'
          inputs:
            azureSubscription: '$(ConnectedServiceName)'
            scriptType: bash
            scriptLocation: inlineScript
            inlineScript: |
              echo "Resource Group: $(ResourceGroupName)"
              echo "Container Instance Name: $(backContainerInstanceNameQA)"
              echo "ACR Login Server: $(acrLoginServer)"
              echo "Image Name: $(backImageName)"
              echo "Image Tag: $(backImageTag)"
              echo "Connection String: $(cnn-string-qa)"

              az container delete --resource-group $(ResourceGroupName) --name $(backContainerInstanceNameQA) --yes

              az container create --resource-group $(ResourceGroupName) \
                --name $(backContainerInstanceNameQA) \
                --image $(acrLoginServer)/$(backImageName):$(backImageTag) \
                --registry-login-server $(acrLoginServer) \
                --registry-username $(acrName) \
                --registry-password $(az acr credential show --name $(acrName) --query "passwords[0].value" -o tsv) \
                --dns-name-label $(backContainerInstanceNameQA) \
                --ports 80 \
                --environment-variables ConnectionStrings__DefaultConnection="$(cnn-string-qa)" \
                --restart-policy Always \
                --cpu $(container-cpu-api-qa) \
                --memory $(container-memory-api-qa)
```

#### 4.2.2 Ejecutar el pipeline y en Azure Portal acceder al recurso de Azure Container Instances creado. Copiar la url del contenedor y navegarlo desde browser. Verificar que traiga datos.

#### 4.2.3 Agregar tareas para generar un recurso Azure Container Instances que levante un contenedor con nuestra imagen de front (DESAFIO)

A la etapa creada en 4.2.1 Agregar tareas para generar contenedor en ACI con nuestra imagen de Front
- Tener en cuenta que el contenedor debe recibir como variable de entorno API_URL el valor de una variable container-url-api-qa definida en nuestro pipeline.
- Para que el punto anterior funcione el código fuente del front debe ser modificado para que la url de la API pueda ser cambiada luego de haber sido construída la imagen. Se deja un ejemplo de las modificaciones a realizar en el repo https://github.com/ingsoft3ucc/CrudAngularConEnvironment.git

#### 4.2.4 Agregar tareas para correr pruebas de integración en el entorno de QA de Back y Front creado en ACI.

### 4.3 Desplegar Imágenes en Azure App Services con Soporte para Contenedores

#### 4.3.1 Agregar a nuestro pipeline una nueva etapa que dependa de nuestra etapa de Construcción y Pruebas y de la etapa de Construcción de Imagenes Docker y subida a ACR

Agregar tareas para crear un recurso Azure App Service que levante un contenedor con nuestra imagen de back utilizando un AppServicePlan en Linux:

```yaml
#---------------------------------------
### STAGE DEPLOY TO AZURE APP SERVICE QA
#---------------------------------------
- stage: DeployImagesToAppServiceQA
  displayName: 'Desplegar Imagenes en Azure App Service (QA)'
  dependsOn:
  - BuildAndTestBackAndFront
  - DockerBuildAndPush
  condition: succeeded()
  jobs:
    - job: DeployImagesToAppServiceQA
      displayName: 'Desplegar Imagenes de API y Front en Azure App Service (QA)'
      pool:
        vmImage: 'ubuntu-latest'
      steps:
        #------------------------------------------------------
        # DEPLOY DOCKER API IMAGE TO AZURE APP SERVICE (QA)
        #------------------------------------------------------
        - task: AzureCLI@2
          displayName: 'Verificar y crear el recurso Azure App Service para API (QA) si no existe'
          inputs:
            azureSubscription: '$(ConnectedServiceName)'
            scriptType: 'bash'
            scriptLocation: 'inlineScript'
            inlineScript: |
              # Verificar si el App Service para la API ya existe
              if ! az webapp list --query "[?name=='$(WebAppApiNameContainersQA)' && resourceGroup=='$(ResourceGroupName)'] | length(@)" -o tsv | grep -q '^1$'; then
                echo "El App Service para API QA no existe. Creando..."
                # Crear el App Service sin especificar la imagen del contenedor
                az webapp create --resource-group $(ResourceGroupName) --plan $(AppServicePlanLinux) --name $(WebAppApiNameContainersQA) --deployment-container-image-name "nginx"  # Especifica una imagen temporal para permitir la creación
              else
                echo "El App Service para API QA ya existe. Actualizando la imagen..."
              fi

              # Configurar el App Service para usar Azure Container Registry (ACR)
              az webapp config container set --name $(WebAppApiNameContainersQA) --resource-group $(ResourceGroupName) \
                --container-image-name $(acrLoginServer)/$(backImageName):$(backImageTag) \
                --container-registry-url https://$(acrLoginServer) \
                --container-registry-user $(acrName) \
                --container-registry-password $(az acr credential show --name $(acrName) --query "passwords[0].value" -o tsv)

              # Establecer variables de entorno
              az webapp config appsettings set --name $(WebAppApiNameContainersQA) --resource-group $(ResourceGroupName) \
                --settings ConnectionStrings__DefaultConnection="$(cnn-string-qa)"
```

---

## 5- Instructivos

### 5.1 Crear un recurso Azure Container Registry

1\. Crear un nuevo recurso en nuestro grupo de recursos

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-22/b9a2ff13-8768-41a9-b94a-e051fdd7fb4d/ascreenshot.jpeg?tl_px=0,0&br_px=859,480&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=309,124)

2\. Type "azure container registry [[enter]]"

3\. Click en Crear Container registry

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-22/7a92bebc-a386-4455-b6ef-32e6d5f2c14a/ascreenshot.jpeg?tl_px=83,356&br_px=1066,905&force_format=jpeg&q=100&width=983&wat_scale=87&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=153,345)

4\. Click the "Nombre del Registro" field.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-22/9e737759-10f4-442d-b7c6-20fbbe7f83cd/ascreenshot.jpeg?tl_px=41,98&br_px=1417,867&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=243,276)

5\. Ingresar un nombre unico para nuestro ACR, como &lt;iniciales&gt;IngSoft3UCCACR

6\. En el combo Plan de Precios seleccionar "Básico"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-22/2aac180a-ae02-4e48-93fa-b2438069feef/ascreenshot.jpeg?tl_px=78,261&br_px=1225,902&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=250,405)

7\. Click "Siguiente: Redes >"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-22/a061f990-29b5-4cfd-8b57-9ead220e0f4b/ascreenshot.jpeg?tl_px=41,149&br_px=1417,918&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=295,600)

8\. Click "Siguiente: Cifrado >"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-22/12d0b212-01d9-4967-84b6-009d2e0e6a22/ascreenshot.jpeg?tl_px=41,149&br_px=1417,918&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=295,600)

9\. Click "Siguiente: Etiquetas >"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-22/b16d170c-d16c-4157-a07f-46353f789e8a/ascreenshot.jpeg?tl_px=41,149&br_px=1417,918&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=295,600)

10\. Click "Siguiente: Revisar y crear >"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-22/1aa47fd4-b7af-4098-890f-5f0cff1034a6/ascreenshot.jpeg?tl_px=39,145&br_px=1424,919&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=294,600)

11\. Click "Crear"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-22/9897db3f-a47a-45f5-8308-54b198446b9a/ascreenshot.jpeg?tl_px=0,4&br_px=1541,966&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=10,632)

12\. Click "Ir al recurso"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-22/c68f069c-652e-454c-9cc0-06cdbaee683e/ascreenshot.jpeg?tl_px=78,65&br_px=1225,706&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=1,182)

13\. Copiar la url de nuestro recurso ACR

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-22/ce058583-1cf6-4e1f-9bb6-e18bb1b4eb35/ascreenshot.jpeg?tl_px=474,62&br_px=1457,611&force_format=jpeg&q=100&width=983&wat_scale=87&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=744,121)

14\. Ir a Configuracion->Claves de acceso de nuestro recurso ACR y tildar el check "Usuario administrador"

![image](https://github.com/user-attachments/assets/8bf99a9a-2479-4e66-8780-091315169b03)

### 5.2 Crear una Service Connection a Azure Resource Manager en Azure DevOps

1\. En nuestro proyecto de ADO, click "Project settings"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-22/e8c26f25-5725-448a-b0df-dd145827ecd4/ascreenshot.jpeg?tl_px=0,4&br_px=1541,966&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=30,653)

2\. Click "Service connections"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-22/ccbfd038-d76e-4a7d-9b6a-d815fd3f406e/ascreenshot.jpeg?tl_px=0,413&br_px=688,798&force_format=jpeg&q=100&width=688&wat_scale=61&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=91,170)

3\. Click "New service connection"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-22/0ccbf282-43b0-47ba-a958-c4c1c02262f9/ascreenshot.jpeg?tl_px=852,0&br_px=1541,384&force_format=jpeg&q=100&width=688&wat_scale=61&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=553,58)

4\. Click "Azure Resource Manager"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-22/16ef2054-72e5-4022-b20a-2ffd82d5b4ae/ascreenshot.jpeg?tl_px=852,18&br_px=1541,403&force_format=jpeg&q=100&width=688&wat_scale=61&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=375,170)

5\. Click "Next"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-22/4a25944a-ae9b-40cb-b370-199c09a9e319/ascreenshot.jpeg?tl_px=852,583&br_px=1541,968&force_format=jpeg&q=100&width=688&wat_scale=61&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=599,322)

6\. Dejar seleccionado el Authentication method por defecto y Click "Next"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-22/f174b447-e513-454d-a85d-9ffca08f5e11/ascreenshot.jpeg?tl_px=474,112&br_px=1457,661&force_format=jpeg&q=100&width=983&wat_scale=87&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=950,201)

7\. Ingresar nuestro mail de Azure Portal

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-22/490f06a3-cd69-42c7-aabc-32195ae98ab7/user_cropped_screenshot.jpeg?tl_px=0,0&br_px=960,599&force_format=jpeg&q=100&width=1073&wat_scale=95&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=562,190)

8\. Click this button.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-22/b97bc19c-9792-4d6d-9cc5-c3163833b4ae/user_cropped_screenshot.jpeg?tl_px=0,0&br_px=960,599&force_format=jpeg&q=100&width=1073&wat_scale=95&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=630,335)

9\. Ingresar contraseña de cuenta de Azure Portal y Click "Sign in"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-22/289ec148-a582-40e9-9229-8685ccad916f/ascreenshot.jpeg?tl_px=100,118&br_px=960,599&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=472,274)

10\. Seleccionar la suscripción y expandir el combo "Resource group"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-22/70f1fed3-5623-4704-a318-8f841e2ac412/ascreenshot.jpeg?tl_px=852,59&br_px=1541,444&force_format=jpeg&q=100&width=688&wat_scale=61&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=476,170)

11\. Seleccionar el grupo de recursos donde creamos nuestro recurso de Azure Container Registry

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-22/f4ea71c0-9fc3-4863-9296-ce772c1c3cce/ascreenshot.jpeg?tl_px=852,141&br_px=1541,526&force_format=jpeg&q=100&width=688&wat_scale=61&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=416,170)

12\. Colocarle un nombre a la Service Connection

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-22/b79d354e-5348-4e9f-979a-e2a0becacb89/ascreenshot.jpeg?tl_px=852,134&br_px=1541,519&force_format=jpeg&q=100&width=688&wat_scale=61&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=417,170)

13\. Type "ServiceConnectionARM"

14\. Click "Grant access permission to all pipelines"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-22/0fbd6fd9-e144-4f92-bc74-29d97a6d5fba/ascreenshot.jpeg?tl_px=852,319&br_px=1541,704&force_format=jpeg&q=100&width=688&wat_scale=61&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=452,170)

15\. Click "Save"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-22/5b693682-d1a3-4fab-b265-20f2903a2bd6/ascreenshot.jpeg?tl_px=852,355&br_px=1541,740&force_format=jpeg&q=100&width=688&wat_scale=61&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=600,170)

### 5.3 Configurar variables secretas en Azure DevOps

1\. En nuestro pipeline Click "Variables"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-22/d7638d53-1aea-4dc0-8c3d-1821f0cd7cb5/ascreenshot.jpeg?tl_px=852,0&br_px=1541,384&force_format=jpeg&q=100&width=688&wat_scale=61&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=516,52)

2\. Click here.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-22/d2dbe776-5623-4662-9d39-adcc6b85b690/ascreenshot.jpeg?tl_px=852,0&br_px=1541,384&force_format=jpeg&q=100&width=688&wat_scale=61&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=611,58)

3\. Type "cnn_string_qa"

4\. Click this text field.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-22/cae06e4c-dcb6-462f-a0dd-f682de8557c4/ascreenshot.jpeg?tl_px=852,0&br_px=1541,384&force_format=jpeg&q=100&width=688&wat_scale=61&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=452,134)

5\. Ingresar la cadena de conexión de nuestra BD de SQL Server

6\. Click "Keep this value secret"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-22/54d4f8d8-740c-407f-9135-fdab8f680639/ascreenshot.jpeg?tl_px=852,0&br_px=1541,384&force_format=jpeg&q=100&width=688&wat_scale=61&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=376,159)

7\. Click "OK"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-22/e706649e-16c2-4da9-82bb-077b3b05d007/ascreenshot.jpeg?tl_px=0,4&br_px=1541,966&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=1054,634)

8\. Click "Save"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-22/23a4eb59-095f-4c76-b0cd-6873b89935bb/ascreenshot.jpeg?tl_px=852,583&br_px=1541,968&force_format=jpeg&q=100&width=688&wat_scale=61&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=616,324)

---

---

---

# El Trabajo Práctico Real – Contenedores en la Nube

## ⚠️ IMPORTANTE

**Este NO es la guía de arriba**. Este es el trabajo práctico que vas a defender oralmente.

---

## 🎯 Enfoque del Trabajo Práctico

Este trabajo práctico es **cloud-agnostic** y **technology-agnostic**.

**Objetivo principal**: Demostrar que comprendés:
- Los conceptos de contenedores y su orquestación
- Servicios de contenedores en la nube (cualquier provider)
- CI/CD con contenedores
- Cómo tomar decisiones arquitectónicas justificadas

**Libertad total de elección**:
- ✅ Podés usar **Azure, AWS, Google Cloud, o servicios gratuitos** (Render, Fly.io, Railway, etc.)
- ✅ Podés usar **cualquier stack tecnológico** (Node.js, Python, Java, Go, .NET, Ruby, etc.)
- ✅ Podés usar **cualquier herramienta de CI/CD** (Azure DevOps, GitHub Actions, GitLab CI, CircleCI, etc.)

---

## Contexto del Trabajo Práctico

Deberás implementar una solución completa de contenedores en la nube para una aplicación de tu elección. La aplicación **debe ser diferente** a la de los ejemplos de la guía

---

## 💰 Recomendación sobre Costos

**NO es necesario gastar dinero para este TP**. Existen alternativas 100% gratuitas:

### Stacks completamente GRATUITOS recomendados:

**Opción 1: GitHub Stack** (100% gratis)
- Container Registry: GitHub Container Registry
- Hosting: Render.com o Fly.io
- CI/CD: GitHub Actions
- Base de datos: Railway o Supabase

**Opción 2: GitLab Stack** (100% gratis)
- Container Registry: GitLab Container Registry
- Hosting: Google Cloud Run (free tier)
- CI/CD: GitLab CI/CD
- Base de datos: PlanetScale o MongoDB Atlas

**Opción 3: Mixed Stack** (100% gratis)
- Container Registry: Docker Hub
- Hosting: Railway.app
- CI/CD: CircleCI
- Base de datos: CockroachDB Serverless

---

## Consignas del Trabajo Práctico

**IMPORTANTE**:
- NO usar la aplicación de la guía
- Usar una aplicación diferente con el stack tecnológico de tu elección, preferentemente usar la que venias usando en los TPs anteriores asi ya te queda casi listo el TP integrador.
- Podés usar servicios de cualquier proveedor cloud (no solo Azure)

### 1. Configuración de Container Registry

**Concepto**: Implementar un registro privado/público de imágenes Docker

**Requisitos**:
- Crear y configurar un Container Registry (ACR, Docker Hub, GitHub CR, GitLab CR, etc.)
- Configurar autenticación y permisos de acceso
- Documentar el proceso de creación y configuración
- Integrar el registry en tu pipeline de CI/CD
- **Justificar la elección** del servicio de registry (¿Por qué elegiste ese y no otro?)

**Ejemplos de servicios**:
- Azure Container Registry (ACR)
- Docker Hub
- GitHub Container Registry (ghcr.io)
- GitLab Container Registry
- Google Container Registry (GCR)
- Amazon Elastic Container Registry (ECR)

### 2. Deploy en Ambiente QA 

**Concepto**: Desplegar tu aplicación en un ambiente de QA/Testing con configuración apropiada

**Requisitos**:
- Elegir un servicio de contenedores en la nube (cualquiera)
- Desplegar tu aplicación (backend y frontend) en QA
- Configurar variables de entorno y secretos de forma segura
- Configurar networking y acceso público (URLs)
- Configurar recursos apropiados para testing (CPU, memoria)
- **Justificar** por qué elegiste ese servicio y esa configuración para QA

**Ejemplos de servicios**:
- Azure: Container Instances, App Services, Container Apps
- AWS: App Runner, Elastic Beanstalk, ECS Fargate
- Google Cloud: Cloud Run, App Engine
- Gratuitos: Render.com, Fly.io, Railway.app, Heroku

### 3. Deploy en Ambiente PROD

**Concepto**: Desplegar tu aplicación en ambiente productivo con configuración apropiada

**Requisitos**:
- Usar el **MISMO servicio** que en QA pero con configuración diferente
- O usar un **servicio DIFERENTE** (justificar el cambio)
- Desplegar tu aplicación en PROD
- Configurar escalabilidad y alta disponibilidad
- Configurar recursos apropiados para producción (CPU, memoria, réplicas)
- Implementar continuous deployment desde tu registry
- **Documentar diferencias** de configuración entre QA y PROD
- **Justificar decisiones**: ¿Por qué esta configuración para PROD?

**Si usás el mismo servicio**:
- Mostrar cómo la configuración difiere entre ambientes
- Explicar estrategia de segregación de ambientes
- Documentar diferencias en recursos, escalabilidad, monitoreo

### 4. Pipeline CI/CD Completo

**Concepto**: Automatizar todo el proceso de build, test y deploy

**Requisitos**:
- Desarrollar pipeline que incluya:
  - Build y test de aplicación
  - Construcción de imágenes Docker optimizadas
  - Push de imágenes a tu registry con versionado (tags)
  - Deploy automático a ambientes QA y PROD
  - Quality gates y aprobaciones manuales entre ambientes


**Ejemplos de herramientas**:
- Azure DevOps Pipelines
- GitHub Actions
- GitLab CI/CD
- CircleCI
- Jenkins
- Travis CI

---

## Arquitectura Mínima Requerida

Tu solución debe incluir los siguientes **conceptos** (podés usar los servicios que quieras):

1. **Container Registry**: Repositorio de imágenes (público o privado)
2. **Ambiente QA**: Deploy de contenedores para testing
3. **Ambiente PROD**: Deploy de contenedores para producción
4. **Pipeline CI/CD**: Automatización completa con la herramienta que elijas
5. **Gestión de Secretos**: Variables secretas correctamente configuradas
6. **Versionado**: Tags en imágenes Docker (no usar solo "latest")
7. **Segregación de Ambientes**: QA y PROD diferenciados (mismo servicio con diferente config, o servicios diferentes)
8. **Documentación**: Justificación de todas las decisiones técnicas

### Ejemplos de Arquitecturas Válidas:

**Ejemplo 1: Mismo servicio, diferente configuración (GitHub + Render - 100% gratis)**
```
GitHub Repo
  → GitHub Actions (CI/CD)
    → Build + Test
    → Docker Build
    → Push to GitHub Container Registry (ghcr.io)
    → Deploy to Render.com QA (1 instancia, 512MB RAM)
    → Approval Gate
    → Deploy to Render.com PROD (2 instancias, 1GB RAM, auto-scaling)
```

**Ejemplo 2: Servicios diferentes (GitLab Stack - 100% gratis)**
```
GitLab Repo
  → GitLab CI/CD
    → Build + Test
    → Docker Build
    → Push to GitLab Container Registry
    → Deploy to Google Cloud Run (QA - free tier, minimal config)
    → Approval Gate
    → Deploy to Render.com (PROD - más control, mejor performance)
```

**Ejemplo 3: Mismo servicio Azure (con créditos estudiantiles)**
```
Azure DevOps Repo
  → Azure Pipelines
    → Build + Test
    → Docker Build
    → Push to Azure Container Registry
    → Deploy to Azure Container Instances QA (1 vCPU, 1.5GB)
    → Approval Gate
    → Deploy to Azure Container Instances PROD (2 vCPU, 4GB, múltiples instancias)
```

**Ejemplo 4: Servicios diferentes Azure (con créditos estudiantiles)**
```
Azure DevOps Repo
  → Azure Pipelines
    → Build + Test
    → Docker Build
    → Push to Azure Container Registry
    → Deploy to Azure Container Instances (QA - rápido, económico para testing)
    → Approval Gate
    → Deploy to Azure App Services (PROD - auto-scaling, monitoring avanzado)
```

**Ejemplo 5: Mixed Stack (100% gratis)**
```
GitHub Repo
  → GitHub Actions
    → Build + Test
    → Docker Build
    → Push to Docker Hub
    → Deploy to Railway.app QA (recursos limitados)
    → Approval Gate
    → Deploy to Railway.app PROD (más recursos, múltiples regiones)
```

---

## Entregables

**⚠️ Recordatorio**: NO usar la aplicación de la guía. Usar tu propia aplicación con el stack que elijas.

### 1. Repositorio actualizado con:
- Código fuente de tu aplicación
- Dockerfiles optimizados para backend y frontend
- Archivos de configuración de CI/CD (YAML, JSON, o el formato que uses)
- Documentación técnica (README.md)

### 2. Documento técnico (PDF o Markdown) que incluya:

#### Sección 1: Decisiones Arquitectónicas y Tecnológicas
- **Stack tecnológico elegido** y justificación (lenguajes, frameworks, librerías)
- **Servicios cloud elegidos** y justificación:
  - ¿Por qué elegiste ese Container Registry?
  - ¿Por qué elegiste ese/esos servicio/s de hosting?
  - ¿Por qué elegiste esa herramienta de CI/CD?
- **Decisión QA vs PROD**:
  - Si usaste el mismo servicio: ¿Por qué? ¿Cómo diferenciás los ambientes?
  - Si usaste servicios diferentes: ¿Por qué? ¿Qué ventajas te da cada uno?
- **Configuración de recursos** (CPU, memoria, instancias, límites) para cada ambiente

#### Sección 2: Implementación

**Container Registry**:
- Evidencia del registry funcionando (capturas con imágenes y tags)
- Configuración de autenticación y permisos

**Ambiente QA**:
- Evidencia de deploy funcionando (capturas, URLs, configuración)
- Variables de entorno y secretos configurados
- Configuración de recursos (CPU, memoria, instancias)

**Ambiente PROD**:
- Evidencia de deploy funcionando (capturas, URLs, configuración)
- Configuración de recursos y escalabilidad
- Continuous deployment configurado
- Diferencias clave con QA (configuración, recursos, seguridad)

**Pipeline CI/CD**:
- Pipeline ejecutándose exitosamente con todas las etapas
- Capturas de cada stage (build, test, push, deploy QA, approval, deploy PROD)

#### Sección 3: Análisis Comparativo

**Tabla comparativa QA vs PROD**:
| Aspecto | QA | PROD | Justificación |
|---------|-----|------|---------------|
| Servicio usado | ... | ... | ... |
| CPU/Memoria | ... | ... | ... |
| Número de instancias | ... | ... | ... |
| Escalabilidad | ... | ... | ... |
| Costos | ... | ... | ... |
| Monitoreo/Logs | ... | ... | ... |

**Si usaste el mismo servicio**:
- Ventajas de usar el mismo servicio para ambos ambientes
- Desventajas o limitaciones encontradas
- Cómo diferenciaste QA de PROD en la práctica

**Si usaste servicios diferentes**:
- Por qué cada servicio es apropiado para cada ambiente
- Trade-offs de la decisión
- Complejidad adicional de manejar dos servicios diferentes

**Análisis de alternativas**:
- Qué otros servicios consideraste
- Por qué no los elegiste
- En qué casos serían mejores opciones

**Análisis de costos** (si aplicás):
- Costos estimados QA vs PROD
- Comparación con alternativas
- Estrategias de optimización

**Escalabilidad a futuro**:
- ¿Cuándo migrarías a Kubernetes?
- ¿Qué cambiarías si la aplicación crece 10x?

#### Sección 4: Reflexión Personal
- **Desafíos encontrados** y cómo los resolviste
- **Qué mejorarías** en una implementación productiva real
- **Aprendizajes clave** del trabajo práctico
- **Comparación** con otros stacks que evaluaste pero no elegiste

### 3. Demo funcional donde puedas mostrar:
- Aplicación funcionando en ambiente QA (con URL funcionando)
- Aplicación funcionando en ambiente PROD (con URL funcionando)
- Diferencias visibles entre QA y PROD (configuración, recursos, etc.)
- Pipeline completo ejecutándose
- Proceso de deployment de cambios (hacer un cambio y mostrar el deploy automático)
- Approval gate entre QA y PROD funcionando
- Rollback en caso de error (opcional pero recomendado)

---

## Criterios de Evaluación

- **Implementación técnica (15%)**: Correcta configuración de todos los servicios cloud elegidos
- **Arquitectura y diseño (15%)**: Decisiones justificadas y apropiadas para el caso de uso
- **Pipeline CI/CD (10%)**: Automatización completa y buenas prácticas
- **Documentación (20%)**: Claridad, completitud y profesionalismo
- **Defensa oral (40%)**: Capacidad de explicar y justificar todas las decisiones tomadas

---

## Preguntas que deberás poder responder en la defensa

### Sobre Decisiones Arquitectónicas:

1. **¿Por qué elegiste ese stack tecnológico específico?** (lenguajes, frameworks)
2. **¿Por qué elegiste esos servicios cloud específicos?** ¿Evaluaste alternativas?
3. **¿Por qué usaste el mismo servicio (o servicios diferentes) para QA y PROD?** ¿Cuáles son las ventajas y desventajas de tu decisión?
4. **¿Cómo diferenciaste QA de PROD?** (recursos, configuración, seguridad)
5. **Si tuvieras presupuesto ilimitado, ¿cambiarías algo?** ¿Y si tuvieras presupuesto cero?
6. **¿Cuándo usarías Kubernetes en lugar de tus servicios elegidos?** Dame ejemplos concretos.

### Sobre Implementación Técnica:

7. **¿Cómo manejás los secretos en tu pipeline?** ¿Por qué de esa forma? ¿Qué alternativas consideraste?
8. **¿Qué estrategia de versionado usás para tus imágenes Docker?** ¿Por qué? ¿Qué pasa con "latest"?
9. **¿Cómo optimizaste tus Dockerfiles?** ¿Multi-stage builds? ¿Capas? ¿Tamaño de imagen?
10. **¿Qué configuración de recursos elegiste para QA y PROD?** ¿Por qué son diferentes (o iguales)?
11. **¿Cómo implementarías un rollback si un deploy falla en producción?** Explicá el proceso paso a paso.

### Sobre CI/CD y DevOps:

12. **¿Por qué elegiste esa herramienta de CI/CD?** ¿Qué alternativas evaluaste?
13. **¿Cómo tu pipeline diferencia entre deploy a QA y PROD?** ¿Variables? ¿Secrets diferentes?
14. **¿Cómo manejarías más entornos (dev/staging)?** ¿Qué cambiarías en tu pipeline?
15. **¿Qué quality gates implementaste?** ¿Por qué esos y no otros?
16. **¿Cómo asegurás que no se deployee código malo a producción?** Hablame de tus estrategias.

### Sobre Seguridad y Mejores Prácticas:

18. **¿Qué consideraciones de seguridad tuviste en cuenta?** En contenedores, en secrets, en networking.
19. **¿Las configuraciones de seguridad son diferentes en QA vs PROD?** ¿Por qué?
20. **¿Cómo manejás las vulnerabilidades en las imágenes base?** ¿Escaneás tus imágenes?
21. **¿Qué logs y monitoreo implementaste?** ¿Cómo debuggearías un problema en producción?
22. **¿Cómo manejás las bases de datos en contenedores?** ¿Persistencia? ¿Backups?
23. **¿Qué harías diferente en una implementación productiva real con usuarios reales?**

### Bonus (muestra comprensión avanzada):

24. **¿Cómo implementarías auto-scaling en tu arquitectura?** ¿Diferente entre QA y PROD?
25. **¿Cómo manejarías disaster recovery?**
26. **¿Qué métricas importantes monitorearías en producción?**
27. **¿Cómo optimizarías los costos de tu solución?**
28. **Si tuvieras que agregar un ambiente de staging, ¿cómo lo harías?**
29. **¿Qué aprendiste sobre contenedores que no sabías antes de este TP?**

---

## Recursos de Ayuda

### Documentación de Contenedores:
- **Docker**: https://docs.docker.com/
- **Docker best practices**: https://docs.docker.com/develop/dev-best-practices/
- **Dockerfile reference**: https://docs.docker.com/engine/reference/builder/
- **Docker Compose**: https://docs.docker.com/compose/

### Documentación Cloud Providers:

**Azure**:
- Documentación oficial: https://docs.microsoft.com/azure/
- Azure CLI reference: https://docs.microsoft.com/cli/azure/
- Azure DevOps pipelines: https://docs.microsoft.com/azure/devops/pipelines/
- Azure Container Instances: https://docs.microsoft.com/azure/container-instances/
- Azure App Services: https://docs.microsoft.com/azure/app-service/

**AWS**:
- AWS documentation: https://docs.aws.amazon.com/
- Amazon ECR: https://docs.aws.amazon.com/ecr/
- AWS App Runner: https://docs.aws.amazon.com/apprunner/
- AWS Elastic Beanstalk: https://docs.aws.amazon.com/elasticbeanstalk/

**Google Cloud**:
- GCP documentation: https://cloud.google.com/docs
- Google Container Registry: https://cloud.google.com/container-registry/docs
- Google Cloud Run: https://cloud.google.com/run/docs
- Google App Engine: https://cloud.google.com/appengine/docs

### Servicios Gratuitos:

**Container Registries**:
- Docker Hub: https://docs.docker.com/docker-hub/
- GitHub Container Registry: https://docs.github.com/packages/working-with-a-github-packages-registry/working-with-the-container-registry
- GitLab Container Registry: https://docs.gitlab.com/ee/user/packages/container_registry/

**Hosting Gratuito**:
- Render: https://render.com/docs
- Fly.io: https://fly.io/docs/
- Railway: https://docs.railway.app/
- Heroku: https://devcenter.heroku.com/

**CI/CD**:
- GitHub Actions: https://docs.github.com/actions
- GitLab CI/CD: https://docs.gitlab.com/ee/ci/
- CircleCI: https://circleci.com/docs/

### Tutoriales y Guías:
- Docker Getting Started: https://docs.docker.com/get-started/
- Kubernetes Basics: https://kubernetes.io/docs/tutorials/kubernetes-basics/
- CI/CD with Docker: https://docs.docker.com/ci-cd/
- Container Security Best Practices: https://snyk.io/learn/container-security/

### Herramientas útiles:
- **Docker Desktop**: Para desarrollo local
- **k3s/minikube**: Para Kubernetes local
- **Dive**: Para analizar capas de imágenes Docker
- **Trivy**: Para escanear vulnerabilidades en imágenes
- **Hadolint**: Linter para Dockerfiles

---

**¡Éxitos con el trabajo práctico!**

Recordá: Lo más importante no es qué servicios usás, sino que entiendas **por qué** los elegiste y **cómo** funcionan.
