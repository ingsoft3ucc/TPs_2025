Configuración: user.name = pmanavella, user.email = pilarmanavella@gmail.com

## Detalle de acciones realizadas hasta el momento

- Se realizó un fork del repositorio original al inicio para tener una copia completa del repositorio original sin perder los permisos para "pushear" cambios.
- Se clonó el fork en la máquina local.
- Se configuraron los datos de usuario de Git:
  - user.name: pmanavella
  - user.email: pilarmanavella@gmail.com
- Se hizo un upstream para mantener mi fork actualizado con el repositorio original, así podré traer los cambios sin romper mi trabajo.
- Se generó una llave SSH ED25519 para autenticación con GitHub.
- Para evitar problemas de red y bloqueo de puerto, se cambió el remoto a HTTPS.
- Se realizó un push exitoso al fork.
- Se sincronizó la rama `main` con el repositorio original (`upstream`) usando:
  - `git fetch upstream`
  - `git merge upstream/main`


<<<<<<< HEAD
## Desarrollo de nueva funcionalidad

- Se creó la rama 'feature/nueva-funcionalidad' para trabajar de manera aislada a la rama main.
- Se creó un nuevo archivo funcionalidad.txt que contendrá las líneas de texto enviadas en cada commit a continuación.
- Se hicieron 2 commits atómicos:
  1. Agregar archivo inicial de la funcionalidad.
  2. Agregar segunda parte de la funcionalidad.
- Esta estrategia permite trazabilidad y facilita revisiones de código en equipo.
=======
## Corrección de error (hotfix)

- Se creó la rama 'hotfix/error-simulacion' desde main.
- Se simuló un error en producción y se aplicó el fix.
- Se hizo commit del fix y se subió al fork.
- Estrategia: merge del hotfix en main y luego en feature/nueva-funcionalidad para mantener sincronización.
>>>>>>> main

