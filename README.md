# Jenkins + Docker 

**Autor:** Angel Allan Francisco Chiquin Gomez
**Carné:** 4090-22-3819
**Curso:** Calidad de Software
**Fecha:** 14/08/2026

## Descripción

Página web básica dockerizada con Nginx y desplegada mediante un pipeline de Jenkins.

## Estructura del repositorio

- `index.html` — página web con los datos del estudiante
- `Dockerfile` — construye la imagen con Nginx Alpine
- `Jenkinsfile` — define el pipeline de CI/CD
- `README.md` — este archivo

## Procedimiento

### 1. Dockerización

Se construyó una imagen ligera basada en `nginx:alpine`, copiando `index.html`
al directorio público del servidor web (`/usr/share/nginx/html`) y exponiendo
el puerto 80. El contenedor arranca Nginx automáticamente al iniciar.

### 2. Verificación local

\`\`\`bash
docker build -t jenkins-docker-chiquin:v1 .
docker run -d --name web-chiquin -p 8081:80 jenkins-docker-chiquin:v1
docker ps
\`\`\`

La aplicación se verificó exitosamente en `http://localhost:8081`.

### 3. Pipeline en Jenkins

Se creó un job tipo Pipeline (`primerpipeline`) configurado para obtener la
definición desde el `Jenkinsfile` almacenado en este repositorio (rama `main`).

El pipeline ejecuta las siguientes etapas:

| Etapa         | Descripción                                                        |
|---------------|---------------------------------------------------------------------|
| Clonación     | Obtiene el repositorio desde GitHub                                |
| Verificación  | Comprueba la existencia de `index.html` y `Dockerfile`             |
| Construcción  | Genera la imagen Docker identificada con el número de build        |
| Despliegue    | Elimina el contenedor previo (si existe) y crea uno nuevo           |
| Confirmación  | Muestra el resultado del proceso (éxito o fallo)                   |

### 4. Actualización

Se modificó el contenido de `index.html`, se hizo push a GitHub y se
re-ejecutó el pipeline, confirmando que la aplicación reflejó el cambio.