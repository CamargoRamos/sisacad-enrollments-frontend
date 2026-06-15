# Sistema de Constancias de Matrícula de Laboratorio - EPIS

Este proyecto es una aplicación web SPA desarrollada con **Vue 3** y **Vite**, diseñada para consultar y generar digitalmente las constancias de matrícula de laboratorio para la Escuela Profesional de Ingeniería de Sistemas (EPIS).

---

##  Despliegue en Producción 

La aplicación se encuentra totalmente desplegada en la nube y puede ser probada a través del siguiente enlace oficial:

**[Ver Constancia de Matrícula en Netlify](https://vocal-vacherin-8533c9.netlify.app/constancia/20250100)**

---
## Comandos Utilizados en el Desarrollo

A continuación se detallan los comandos ejecutados en la terminal de forma secuencial para la creación, configuración y construcción del proyecto:

### 1. Inicialización del Proyecto
Para crear la estructura base de la aplicación con Vue 3 y Vite en el directorio actual:

```sh
npm create vue@latest .
```
### 2. Instalación de Dependencias
Para instalar los paquetes del núcleo de Vue y la librería Axios para las peticiones HTTP a la API:

```sh
npm install
npm install axios
```
### 3. Entorno de Desarrollo (Local)
Para levantar el servidor de desarrollo local y probar los cambios en tiempo real:

```sh
npm run dev
```
### 4. Compilación para Producción
Para empaquetar, minificar y generar la carpeta de distribución final (dist) con el archivo _redirects incluido antes de subirlo a Netlify:

```sh
npm run build
```
### 5. Respaldo en el repositorio
Para guardar los cambios de forma local en Git y subirlos al repositorio remoto de GitHub:

```sh
git add .
git commit -m "Feat: Frontend"
git branch -M main
git push -u origin main
```
