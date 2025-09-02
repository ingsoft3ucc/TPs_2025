## Trabajo Práctico 5 - Despliegue de aplicaciones con Azure Devops Release Pipelines

### 1- Objetivos de Aprendizaje
 - Adquirir conocimientos acerca de las herramientas de despliegue y releases de aplicaciones.
 - Configurar este tipo de herramientas.
 - Comprender el concepto de recurso en Azure
 - Comprender los conceptos básicos de Release Pipelines en Azure DevOps.
 - Configurar un Release Pipeline para automatizar despliegues en diferentes entornos-

### 2- Unidad temática que incluye este trabajo práctico
Este trabajo práctico corresponde a la unidad Nº: 3 (Libro Continuous Delivery: Cap 10)

### 3- Consignas a desarrollar en el trabajo práctico:
 - Los despliegues (deployments) de aplicaciones se pueden realizar en diferentes tipos de entornos
   - On-Premise (internos) es decir en servidores propios.
   - Nubes Públicas, ejemplo AWS, Azure, Gcloud, etc.
   - Plataformas como servicios (PaaS), ejemplo Heroku, Google App Engine, AWS, Azure WebApp, etc
 - En este práctico haremos despliegue a Plataforma como Servicio utilizando Azure Web Apps

### 4- Desarrollo:
4.1\. Crear una cuenta en Azure

4.2\. Crear un recurso Web App en Azure Portal y navegar a la url provista

4.3\. Actualizar Pipeline de Build para que use tareas de DotNetCoreCLI@2 como en el pipeline clásico, luego crear un Pipeline de Release en Azure DevOps con CD habilitada

4.4\. Optimizar Pipeline de Build

4.5\. Verificar el deploy en la url de la WebApp /weatherforecast

4.6\. Realizar un cambio al código del controlador para que devuelva 7 pronósticos, realizar commit, evaluar ejecución de pipelines de build y release, navegar a la url de la webapp/weatherforecast y corroborar cambio

4.7\. Clonar la Web App de QA para que contar con una WebApp de PROD a partir de un Template Deployment en Azure Portal y navegar a la url provista para la WebApp de PROD.

4.8\. Agregar una etapa de Deploy a Prod en Azure Release Pipelines 

4.9\.  Realizar un cambio al código del controlador para que devuelva 10 pronósticos, realizar commit, evaluar ejecución de pipelines de build y release, navegar a la url de la webapp/weatherforecast y corroborar cambio, verificar que en la url de la webapp_prod/weatherforecast se muestra lo mismo.

4.10\. Modificar pipeline de release para colocar una aprobación manual para el paso a Producción.

4.11\. Realizar un cambio al código del controlador para que devuelva 5 pronósticos, realizar commit, evaluar ejecución de pipelines de build y release, navegar a la url de la webapp/weatherforecast y corroborar cambio, verificar que en la url de la webapp_prod/weatherforecast aun se muestra la versión anterior.

4.12\. Aprobar el pase ya sea desde el release o desde el mail recibido. 
<img width="1197" alt="image" src="https://github.com/user-attachments/assets/05e8d1a1-ae06-4824-91d7-1a6e0162e859">

<img width="1221" alt="image" src="https://github.com/user-attachments/assets/33710115-a5c3-43ee-a208-5879331c7a8d">

<img width="1466" alt="image" src="https://github.com/user-attachments/assets/9655c548-d3da-4d29-8080-8324d711c18f">

4.12.1\. Notar que se puede dar la aprobación pero posponer su aplicación hasta una determinada fecha


<img width="1222" alt="image" src="https://github.com/user-attachments/assets/11f710b9-4ac1-4e2c-a560-7714835b2ce6">

4.13\. Esperar a la finalización de la etapa de Pase a Prod y luego corroborar que en la url de la webapp_prod/weatherforecast se muestra la nueva versión coinicidente con la de QA.
<img width="1465" alt="image" src="https://github.com/user-attachments/assets/533ac589-58e1-4f36-9356-d37a0da58220">

<img width="1125" alt="image" src="https://github.com/user-attachments/assets/26256e9d-28c5-41d7-97cb-e86df429e817">

4.14\. Realizar un pipeline (no release) que incluya el deploy a QA y a PROD con una aprobación manual. El pipeline debe estar construido en YAML sin utilizar el editor clásico de pipelines ni el editor clásico de pipelines de release.
 
### 5- Instructivos:
#### 5.1 Crear una cuenta en Azure

5.1.1\. Navigate to [https://azure.microsoft.com/es-mx/](https://azure.microsoft.com/es-mx/)


5.1.2\. Click "Comenzar a usar Azure"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-08-29/c6b10b19-339c-4d02-a524-eb552d66fda4/ascreenshot.jpeg?tl_px=681,0&br_px=1541,480&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=558,-3)


5.1.3\. Click "Probar Azure gratis"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-08-29/ac523580-203f-4497-88c6-e2d05e20102b/ascreenshot.jpeg?tl_px=0,204&br_px=859,685&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=192,212)


5.1.4\. Click "Usar otra cuenta"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-08-29/80d9b3a9-9928-4894-8044-01fdd370471e/ascreenshot.jpeg?tl_px=269,443&br_px=1129,924&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


5.1.5\. Click this button.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-08-29/f32d5e77-531d-4364-80ef-0c8bbb379c05/ascreenshot.jpeg?tl_px=456,312&br_px=1316,793&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


5.1.6\. Click "Cree una"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-08-29/5e60ec4c-8f1e-4a79-bbd4-ce5c48057e28/ascreenshot.jpeg?tl_px=337,245&br_px=1197,726&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


5.1.7\. Click "get a new one"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-08-29/afa2e4f5-8d1e-44e1-8660-12de7d97a93e/ascreenshot.jpeg?tl_px=365,162&br_px=1225,643&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


5.1.8\. Click "Next"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-08-29/ac07a53d-3685-48a7-ad85-c52a04dabb7e/ascreenshot.jpeg?tl_px=458,336&br_px=1318,817&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


5.1.9\. Click "Next"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-08-29/1c7816d2-921b-492d-9ed2-cc1b2efc7917/ascreenshot.jpeg?tl_px=462,368&br_px=1322,849&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


5.1.10\. Click "Next"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-08-29/c8abc064-5ed4-4956-b86a-1e4b8a0c7637/ascreenshot.jpeg?tl_px=497,372&br_px=1357,853&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


5.1.11\. Click "Next"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-08-29/28558c68-72c2-4689-9d48-337ddf893843/ascreenshot.jpeg?tl_px=500,444&br_px=1360,925&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


5.1.12\. Click "To verify your email address use this security code: 399365"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-08-29/fbe2a0bd-0b90-474a-a3d3-dde4ffb0a52f/ascreenshot.jpeg?tl_px=243,172&br_px=1103,653&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


5.1.13\. Click "Next"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-08-29/56490255-84b9-4921-9819-6ab6d10edae8/ascreenshot.jpeg?tl_px=482,417&br_px=1342,898&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


5.1.14\. Click "Sign in"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-08-29/d06b46be-fb5c-454a-aac5-2091b8f84df4/ascreenshot.jpeg?tl_px=462,360&br_px=1322,841&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


5.1.15\. Click "Yes"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-08-29/b69e6757-6a9f-448f-9509-325ebef29b71/ascreenshot.jpeg?tl_px=455,341&br_px=1315,822&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


5.1.16\. Click "Acepto el contrato de cliente."

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-08-29/72a45337-d58b-4413-b235-688b9b02634b/ascreenshot.jpeg?tl_px=0,286&br_px=859,767&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=227,212)


5.1.17\. Click the "Teléfono" field.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-08-29/54267126-b1d8-4591-8172-fd3bf32ae789/ascreenshot.jpeg?tl_px=14,96&br_px=873,577&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


5.1.18\. Click "Envíeme un mensaje de texto"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-08-29/75b93bcb-b64f-4353-9c2b-49a6c5113bd4/ascreenshot.jpeg?tl_px=0,270&br_px=859,751&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=390,212)


5.1.19\. Click "Comprobar código"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-08-29/29b45c8e-5b26-4a7b-8367-af897209302a/ascreenshot.jpeg?tl_px=0,401&br_px=859,882&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=299,212)


5.1.20\. Click "Siguiente"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-08-29/a2e5ee85-1a17-40c8-a1f9-fc8507276d5a/ascreenshot.jpeg?tl_px=0,487&br_px=859,968&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=290,217)


5.1.21\. Click this button field.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-08-29/87dc3039-4f21-4cc4-bc78-edffbb5d82e7/ascreenshot.jpeg?tl_px=0,101&br_px=859,582&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=373,212)


5.1.22\. Click this text field.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-08-29/2547ec3f-0c04-4f44-9aec-0733a87eff6d/ascreenshot.jpeg?tl_px=0,356&br_px=859,837&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=359,212)


5.1.23\. Click "Registrarse"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-08-29/03b09f0c-05be-43bf-9910-df3f84044488/ascreenshot.jpeg?tl_px=116,487&br_px=976,968&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,361)


5.1.24\. Click "Siguiente"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-08-29/99adbd68-9ce5-4e97-8b40-a54f7ff5b20f/ascreenshot.jpeg?tl_px=467,431&br_px=1327,912&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


5.1.25\. Click "Regístrese para obtener Azure con los precios de pago por uso."

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-08-29/e0e34b63-d502-4239-82dd-b615dc9e05c5/ascreenshot.jpeg?tl_px=423,134&br_px=1283,615&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


5.1.26\. Click "Acepto el contrato de cliente."

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-08-29/718b7a57-fa94-47ad-962f-a0a96767c8d8/ascreenshot.jpeg?tl_px=0,61&br_px=859,542&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=231,212)


5.1.27\. Click "Siguiente"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-08-29/6617b10e-3771-4408-bbe1-61e21124c60c/ascreenshot.jpeg?tl_px=0,286&br_px=859,767&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=294,212)


5.1.28\. Click "Siguiente"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-08-29/5c569c66-3721-45fe-8843-273cf8c1359a/ascreenshot.jpeg?tl_px=0,290&br_px=859,771&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=274,212)


5.1.29\. Click "Registrarse"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-08-29/7f1b810e-6dfd-4248-9ac6-6e5f10499494/ascreenshot.jpeg?tl_px=28,487&br_px=887,968&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,361)


5.1.30\. Click "Siguiente"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-08-29/62c8a8b0-2a17-452f-80b0-f0267bb9bb53/ascreenshot.jpeg?tl_px=469,441&br_px=1329,922&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


5.1.31\. Click here.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-08-29/84c7cb19-c812-44a4-9dec-d68c0be415e0/ascreenshot.jpeg?tl_px=454,256&br_px=1314,737&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


5.1.32\. Click here.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-08-29/13d09b79-3b01-4c5b-85ee-3c9e5ab7fc63/ascreenshot.jpeg?tl_px=0,0&br_px=859,480&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=134,137)


5.1.33\. Click "Verify"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-08-29/a0abf4e1-adf8-4b58-bf0a-60c917564c77/ascreenshot.jpeg?tl_px=466,373&br_px=1326,854&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


5.1.34\. Click "[azportal2024@gmail.com](mailto:azportal2024@gmail.com)"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-08-29/a52e29ff-3449-48e5-a598-5f0b4c6885cb/ascreenshot.jpeg?tl_px=338,125&br_px=1198,606&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


5.1.35\. Click this icon.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-08-29/35588b40-0ab1-4a69-bf1e-6f8738eafd30/ascreenshot.jpeg?tl_px=681,0&br_px=1541,480&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=808,42)


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


#### 5.3 Crear un Pipeline de Release en Azure DevOps

5.3.1\. Navegar a [https://dev.azure.com/](https://dev.azure.com/azportal2024/Sample01)NOMBRE_DE_TU_ORGANIZACION


5.3.2\. Seleccionar el Proyecto deseado


5.3.3\. Click "Pipelines"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/10bfa9ac-c36c-4411-b517-f45b2d5f165f/ascreenshot.jpeg?tl_px=0,143&br_px=859,624&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=83,212)


5.3.4\. Click "Releases"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/f71931be-da25-48b1-af65-0663113baf8d/ascreenshot.jpeg?tl_px=0,155&br_px=859,636&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=85,212)


5.3.5\. Click "New pipeline"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/556f67d2-c5f2-4624-ac84-22c94661711e/ascreenshot.jpeg?tl_px=482,267&br_px=1342,748&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


5.3.6\. Seleccionar template "Azure App Service Deployment" y click en "Apply"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/610cdcbb-c913-40b9-b87d-9d9c24a00880/ascreenshot.jpeg?tl_px=681,0&br_px=1541,480&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=747,199)


5.3.7\. Cambiar el nombre de la etapa a "Deploy a QA"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/e19d9ccf-b556-48d2-9f8f-3a00a7737863/ascreenshot.jpeg?tl_px=681,94&br_px=1541,575&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=420,212)


5.3.8\. Click here.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/509b054f-6498-45f6-a8ec-1bd72c6f970f/ascreenshot.jpeg?tl_px=681,0&br_px=1541,480&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=816,129)


5.3.9\. Agregar el artefacto a deployear, es el resultado del pipeline de build. Click en "Add"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/a1915d11-b8d6-44b7-8c0c-dcf46530b305/ascreenshot.jpeg?tl_px=17,0&br_px=877,480&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,189)


5.3.10\. Seleccionamos el proyecto (por defecto el proyecto en el que estamos) y seleccionamos el pipeline del cual queremos sacar el artefacto a deployear. Click en "Expand"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/39cc8234-57b2-4e2b-823d-82980637b6f1/ascreenshot.jpeg?tl_px=681,160&br_px=1541,641&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=771,212)


5.3.11\. Click en el nombre de nuestro pipeline

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/4b7dd169-a457-4b37-a9fa-588edffd70c4/ascreenshot.jpeg?tl_px=681,193&br_px=1541,674&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=566,212)


5.3.12\. Click "Add"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/cf87276c-4e51-4837-a7ec-b25b95deba0f/ascreenshot.jpeg?tl_px=536,433&br_px=1396,914&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


5.3.13\. Habilitamos Continuous Deployment, cada vez que el pipeline de build se ejecute exitosamente, se disparará este pipeline de release.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/6514f4d1-91a4-4ee8-8d47-f637f3efa17b/ascreenshot.jpeg?tl_px=31,47&br_px=890,528&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


5.3.14\. Habilitamos el toggle dejandolo **Habilitado**

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/e222b441-b840-4afb-bf1e-9bd27c5204d0/ascreenshot.jpeg?tl_px=498,6&br_px=1358,487&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


5.3.15\. Configuramos las tareas del stage. Click "1 job, 1 task"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/70a7bffa-dd25-416e-abd0-fddc69097ec4/ascreenshot.jpeg?tl_px=275,97&br_px=1135,578&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


5.3.16\. Buscamos nuestra suscripción creada en Azure Portal. Click "Expand"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/3270900a-3e9d-494e-bf0f-17904e91ff36/ascreenshot.jpeg?tl_px=681,62&br_px=1541,543&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=746,212)


5.3.17\. Seleccionamos nuestra suscripción.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/829c7cc7-fffc-45cc-8c38-2420a1219fff/ascreenshot.jpeg?tl_px=681,159&br_px=1541,640&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=645,212)


5.3.18\. Click "Authorize". Nos pedirá nuestros datos de la cuenta de Azure Portal

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/cf53903b-a799-426b-9301-55945b4037a6/ascreenshot.jpeg?tl_px=681,61&br_px=1541,542&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=691,212)


5.3.19\. Ingresamos nuestra cuenta de Azure Portal y hacemos click en "Siguiente"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/9af18179-fcf7-48c4-972f-bb9efd871973/ascreenshot.jpeg?tl_px=100,90&br_px=960,571&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=482,212)


5.3.20\. Click "Send code"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/d2d6df7b-91ca-4e41-8b6b-6e8acd31fa56/ascreenshot.jpeg?tl_px=100,118&br_px=960,599&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=487,278)


5.3.21\. Una vez recibido el código en nuestro mail, lo ingresamos y clickeamos en "Sign in"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/a4a33354-dce0-429d-b86b-e44c800dccd8/ascreenshot.jpeg?tl_px=100,118&br_px=960,599&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=468,285)


5.3.22\. Expandimos el combo para ver las opciones disponibles y dejamos seleccionada "Web App on Windows"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/4e07dfc5-5cec-4685-b30b-02bed1c62530/ascreenshot.jpeg?tl_px=681,187&br_px=1541,668&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=756,212)


5.3.23\. Expandimos el combo de App service Name y seleccionamos nuestra Web App creada en Azure Portal

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/5fb87794-6e23-470c-8faf-6e378ec76225/ascreenshot.jpeg?tl_px=681,284&br_px=1541,765&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=704,212)


5.3.24\. Click "Save"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/7c796430-b452-42e0-a2bd-2aae924fdca4/ascreenshot.jpeg?tl_px=681,0&br_px=1541,480&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=447,52)


5.3.25\. Click "OK"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/748513ed-d926-44da-8e6c-d8672e57486d/ascreenshot.jpeg?tl_px=451,360&br_px=1311,841&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


5.3.26\. Click here.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/b0aa6a68-675a-41e4-9af4-0b1118849b8b/ascreenshot.jpeg?tl_px=183,76&br_px=1043,557&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)


5.3.27\. Click "Releases"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/e667c53d-66e1-4d86-903b-d99c088f0574/ascreenshot.jpeg?tl_px=0,157&br_px=859,638&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=83,212)


5.3.28\. Seleccionamos nuestro pipeline de Release

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/1f44eebe-623a-4d46-ba80-dbbda219378f/ascreenshot.jpeg?tl_px=0,0&br_px=859,480&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=396,152)


5.3.29\. Click here.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/cd6fb89c-cec9-48ab-b8dd-66c5418c00a7/ascreenshot.jpeg?tl_px=681,0&br_px=1541,480&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=776,107)


5.3.30\. Clickeamos en Rename para renombrar nuestro pipeline de Release

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/eb6567ff-a9b7-47f5-87a5-ab7245d01c48/ascreenshot.jpeg?tl_px=681,0&br_px=1541,480&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=656,140)


5.3.31\. Type "ReleasePipeline01"


5.3.32\. Click "OK"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/d8c2f803-ea8f-44a9-b99e-7b6e5b3c92b5/ascreenshot.jpeg?tl_px=558,322&br_px=1418,803&force_format=jpeg&q=100&width=860&wat_scale=76&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=402,212)

#### 5.4 Clonar una Web App a partir de un Template Deployment en Azure Portal

5.5.1\. Navigate to [https://portal.azure.com/#home](https://portal.azure.com/#home)


5.5.2\. Click en el Grupo de Recursos

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/6264b362-a4c8-4b2b-84b6-8db640b42c74/ascreenshot.jpeg?tl_px=0,0&br_px=2182,1664&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=178,454)


5.5.3\. Crear un nuevo recurso

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/b6873b5f-643b-4333-a7e8-082034aa178b/ascreenshot.jpeg?tl_px=0,0&br_px=1719,961&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=400,169)


5.5.4\. Buscar el recurso "Template Deployment (implementar mediante plantillas personalizadas)" y seleccionarlo

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/1c2f28d6-9d90-4eae-8787-8e2c2d5c6674/ascreenshot.jpeg?tl_px=0,565&br_px=1719,1526&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=371,276)


5.5.5\. Click "Crear"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/14e4d429-e482-42bb-9ed2-8767fd40c1fe/ascreenshot.jpeg?tl_px=19,127&br_px=1739,1088&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=524,277)


5.5.6\. Click "Cree su propia plantilla en el editor."

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/27bdd534-57f4-4174-b3df-783e0a7a1746/ascreenshot.jpeg?tl_px=0,59&br_px=1719,1020&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=300,277)


5.5.7\. Descomprimir el archivo template.zip que se descargó en los pasos 31 y 32 del instructivo "**Crear una Web App en Azure Portal"**

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/9c9eebbf-f5d5-44f2-a49e-0661b5060e8a/screenshot.jpeg?tl_px=0,0&br_px=1522,996&force_format=jpeg&q=100&width=1120.0)


5.5.8\. Cargamos el archivo template.json que se extrajo del archivo template.zip que se descargó en los pasos 31 y 32 del instructivo "**Crear una Web App en Azure Portal"**

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/0ef39e7d-5e48-44be-9bdb-4c06e96d7e0a/ascreenshot.jpeg?tl_px=0,0&br_px=1719,961&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=461,156)


5.5.9\. Click "Guardar"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/a9263d36-b7d1-46fb-b299-2e8eabf79e30/ascreenshot.jpeg?tl_px=0,0&br_px=2182,1664&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=40,791)


5.5.10\. Click "Editar parámetros"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/51078d96-f220-416c-860f-969f4e91969c/ascreenshot.jpeg?tl_px=0,0&br_px=2182,1281&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=502,286)


5.5.11\. Cargamos el archivo parameters.json que se extrajo del archivo template.zip que se descargó en los pasos 31 y 32 del instructivo "**Crear una Web App en Azure Portal"**

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/4504a6c3-50c6-4746-8615-301e5461f840/ascreenshot.jpeg?tl_px=0,0&br_px=1719,961&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=107,165)


5.5.12\. Click "Guardar"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/7c5f7826-e42e-4b3a-b08e-6dc3ab5bd16c/ascreenshot.jpeg?tl_px=0,702&br_px=1719,1664&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=43,550)


5.5.13\. Click "Editar parámetros"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/4a68dfcf-97e1-458f-9581-ae1f5e5488a3/ascreenshot.jpeg?tl_px=168,167&br_px=1887,1128&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=524,277)


5.5.14\. Cambiamos el nombre de nuestra Web App agregandole el sufijo "-PROD"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/17b563b6-bb8b-4d12-92df-7db192f5039f/ascreenshot.jpeg?tl_px=0,293&br_px=1719,1254&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=315,277)


5.5.15\. Type "-PROD"


5.5.16\. Click em Guardar

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/4aa5598f-03e1-4ef9-82ac-57d74b060c6f/user_cropped_screenshot.jpeg?tl_px=0,382&br_px=2182,1664&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=14,603)


5.5.17\. Click "Siguiente"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/834d9d7b-89fe-436e-8d6d-1dba404a0c1c/ascreenshot.jpeg?tl_px=0,0&br_px=2182,1664&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=160,782)


5.5.18\. Click "Crear"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/88709564-ee13-449c-9603-7e2c88ab219d/ascreenshot.jpeg?tl_px=0,0&br_px=2182,1664&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=257,787)


5.5.19\. Click "Ir al grupo de recursos"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/289c3cc8-4754-4ec0-b5e3-c7bdc65648af/ascreenshot.jpeg?tl_px=0,0&br_px=2182,1664&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=380,446)


5.5.20\. Actualizamos el Grupo de Recursos

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/61ee0cc7-7936-4a4f-bae5-91a842c35817/ascreenshot.jpeg?tl_px=0,0&br_px=2182,1281&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=713,113)


5.5.21\. Vemos que hemos clonado la WebApp correctamente:

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/44d1178a-26c3-43aa-b48e-5f2bc79b7c5a/user_cropped_screenshot.jpeg?tl_px=314,25&br_px=2607,1306&force_format=jpeg&q=100&width=1120.0)

#### 5.5 Agregar una etapa de Deploy a Prod en Azure Release Pipelines 

5.5.1\. Navegar a  [https://dev.azure.com/](https://dev.azure.com/ingsoft3ucc/Sample02/_release?_a=releases&view=mine&definitionId=3)NOMBRE_ORGANIZACION


5.5.2\. Click "Pipelines"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/7a0c963c-c702-4078-9480-55734f2387c0/ascreenshot.jpeg?tl_px=0,0&br_px=2182,1281&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=55,209)


5.5.3\. Click "Releases"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/f1126322-76e6-48e8-b404-4353ec24fdc6/ascreenshot.jpeg?tl_px=0,265&br_px=1547,1130&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=89,277)


5.5.4\. Click "Edit"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/5444efc1-4239-4afb-baf4-ab14c526db56/ascreenshot.jpeg?tl_px=634,0&br_px=2182,865&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=722,86)


5.5.5\. Click en "Clone".

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/4af167d3-0efc-4870-be03-da87c08f3f5b/ascreenshot.jpeg?tl_px=536,283&br_px=2083,1148&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=524,277)


5.5.6\. Click aqui.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/a4161249-f801-4fa7-8f03-3a92eba1ccbb/ascreenshot.jpeg?tl_px=634,142&br_px=2182,1007&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=819,277)


5.5.7\. Renombrar la etapa a "Deploy a Prod".

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/074968ec-28e0-4f1b-be50-164e1434cbbd/ascreenshot.jpeg?tl_px=634,175&br_px=2182,1040&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=644,277)


5.5.8\. Click here.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/5ffe3f11-eb4f-45fc-a128-eef170efe723/ascreenshot.jpeg?tl_px=634,0&br_px=2182,865&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=1063,168)


5.5.9\. Click "1 job, 1 task"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/13046f56-19b8-4b89-8e95-f5a74ff6757c/ascreenshot.jpeg?tl_px=634,177&br_px=2182,1042&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=764,277)


5.5.10\. Expandimos la lista de Web Apps

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/fe4fce2d-6e65-41a1-9e84-de76556e6831/ascreenshot.jpeg?tl_px=634,432&br_px=2182,1297&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=985,277)


5.5.11\. Seleccionar nuestra WebApp de PROD

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/2bc1dfeb-7b98-4230-97ed-8e66612698d5/ascreenshot.jpeg?tl_px=634,529&br_px=2182,1394&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=860,277)


5.5.12\. Click "Azure App Service deploy"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/d8bce40b-6abf-407b-8310-87164da7a94b/ascreenshot.jpeg?tl_px=331,90&br_px=1878,955&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=524,277)


5.5.13\. Click "Save"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/60dd21e8-96c9-4917-95ba-c1dd8f5090d4/ascreenshot.jpeg?tl_px=634,0&br_px=2182,865&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=726,60)


5.5.14\. Click "OK"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/349c21f3-f5c9-4321-ae32-900ec1af8697/ascreenshot.jpeg?tl_px=484,547&br_px=2031,1412&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=524,277)

####5.6 Agregar Aprobación Manual para el Paso a Prod en un Azure Release Pipeline

5.6.1\. Navegar a [https://dev.azure.com/ ](https://dev.azure.com/ingsoft3ucc/Sample02/_build)NOMBRE_ORGANIZACION


5.6.2\. Click "Releases"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/4c1f29ac-05c1-4eec-8ce6-33558b87f5c6/ascreenshot.jpeg?tl_px=0,279&br_px=1547,1144&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=101,277)


5.6.3\. Click en Edit.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/9398590b-c897-4b60-8558-525e39f6074b/ascreenshot.jpeg?tl_px=634,0&br_px=2182,865&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=717,83)


5.6.4\. Click en "Pre-deployment conditions"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/b538eb66-cb9b-488c-86ed-64ca48a47cbc/ascreenshot.jpeg?tl_px=634,177&br_px=2182,1042&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=715,277)


5.6.5\. Habilitar las "Pre-deployment conditions"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/fe1bf373-7760-4958-b4b2-dcfcf1a6c21f/ascreenshot.jpeg?tl_px=634,798&br_px=2182,1664&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=928,283)


5.6.6\. Click en "Search users and groups for approvers" para agregar a los usuarios que pueden aprobar el pase a prod.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/a76d1832-5c2b-4a45-b56d-574a10a33923/ascreenshot.jpeg?tl_px=634,353&br_px=2182,1218&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=614,277)


5.6.7\. Seleccionar un usuario

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/75f34bd8-1e7b-4cba-afea-d4dbbf55f9e1/ascreenshot.jpeg?tl_px=634,403&br_px=2182,1268&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=601,277)


5.6.8\. Ver que siginifica la propiedad Timeout

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/6fdfbcc4-541b-40e4-8592-1bce10717be6/ascreenshot.jpeg?tl_px=521,418&br_px=2068,1283&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=523,276)


5.6.9\. Click here.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/48f71632-8c55-42b4-ade5-612184abf539/ascreenshot.jpeg?tl_px=634,0&br_px=2182,865&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=1057,158)


5.6.10\. Click "Save"

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/033c1a25-7ac8-4b8d-81e9-a22588d2e1b7/ascreenshot.jpeg?tl_px=634,0&br_px=2182,865&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=734,62)


5.6.11\. Click the "Comment" field.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/9526005e-48b0-43dc-8ba4-cf138807b1a8/ascreenshot.jpeg?tl_px=491,375&br_px=2038,1240&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=524,277)


5.6.12\. Type "Se agrega aprobacion manual"


5.6.13\. Click here.

![](https://ajeuwbhvhr.cloudimg.io/colony-recorder.s3.amazonaws.com/files/2024-09-01/601a17d6-c1e2-4044-a2c9-0e60bd8cbc36/ascreenshot.jpeg?tl_px=487,520&br_px=2034,1385&force_format=jpeg&q=100&width=1120.0&wat=1&wat_opacity=0.7&wat_gravity=northwest&wat_url=https://colony-recorder.s3.us-west-1.amazonaws.com/images/watermarks/FB923C_standard.png&wat_pad=524,277)


### 6-  Presentación del trabajo práctico.
- Subir un doc al repo con las capturas de pantalla de los pasos realizados y tener en el excel de repos (https://docs.google.com/spreadsheets/d/1mZKJ8FH390QHjwkABokh3Ys6kMOFZGzZJ3-kg5ziELc/edit?gid=0#gid=0) la url del proyecto de AzureDevops.
- Aclarar los nombres de los pipelines que se deben evaluar.

### 7-  Criterio de Calificación
Los pasos 4.1 al 4.13 representan un 60% de la nota total, los pasos 4.13 y subsiguientes representan el 40% restante.

### 8-  Documentación y Recursos Adicionales
- https://learn.microsoft.com/en-us/azure/devops/?view=azure-devops

- https://learn.microsoft.com/en-us/azure/devops/pipelines/?view=azure-devops

- https://learn.microsoft.com/en-us/azure/devops/pipelines/yaml-schema/?view=azure-pipelines

- https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/overview

- https://learn.microsoft.com/en-us/azure/azure-resource-manager/templates/overview


