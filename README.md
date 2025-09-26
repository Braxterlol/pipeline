# ProyectoMD - CI/CD con Docker y AWS

## 1. Breve explicación del pipeline

Este proyecto utiliza un pipeline de CI/CD configurado con **GitHub Actions** para automatizar la construcción, publicación y despliegue de la aplicación Node.js en un entorno en la nube (AWS EC2).  

El flujo principal del pipeline es el siguiente:

1. Se realiza un **push** o se crea un **Pull Request** en la rama `develop`.
2. GitHub Actions realiza un **checkout** del repositorio.
3. Construye la **imagen Docker** de la aplicación.
4. Realiza **login en Docker Hub** y sube la imagen al repositorio `braxter/myapp`.
5. Se conecta por **SSH a la instancia EC2** y ejecuta los comandos de despliegue:
   - Hace `docker pull` de la imagen más reciente.
   - Detiene y elimina el contenedor anterior.
   - Levanta un nuevo contenedor con la aplicación.
6. Ejecuta las **migraciones de base de datos** automáticamente al momento del despliegue.

---

## 2. Descripción de cómo se manejan las migraciones

Las migraciones se gestionan mediante **Knex.js** con un contenedor MySQL separado.  

- La configuración de Knex se encuentra en `knexfile.js`.
- Los scripts de migración se encuentran en `src/migrations`.
- Al desplegar, se ejecuta el comando:

```bash
npx knex migrate:latest --knexfile ./knexfile.js
```
## conclusiones personales
Usar CI/CD con contenedores permite un despliegue reproducible y rápido, eliminando inconsistencias entre ambientes locales y producción.

La integración con AWS EC2 facilita la automatización del despliegue sin depender de intervención manual.

Las migraciones automatizadas con Knex aseguran que la base de datos se mantenga consistente con cada nueva versión de la aplicación.

Este flujo reduce errores humanos, acelera el ciclo de desarrollo y permite escalar fácilmente el proyecto en la nube.

Aprendí la importancia de sincronizar ramas y configurar correctamente permisos de Docker para evitar errores en producción.
