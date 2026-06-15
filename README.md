# Sistema de Constancias de Matrícula de Laboratorio - EPIS

Este proyecto es una aplicación web SPA desarrollada con **Vue 3** y **Vite**, diseñada para consultar y generar digitalmente las constancias de matrícula de laboratorio para la Escuela Profesional de Ingeniería de Sistemas (EPIS).

---

## 🚀 Despliegue en Producción (Live Demo)

La aplicación se encuentra totalmente desplegada en la nube y puede ser probada a través del siguiente enlace oficial:

👉 **[Ver Constancia de Matrícula en Netlify](https://vocal-vacherin-8533c9.netlify.app/constancia/20250100)**

---

## 🛠️ Soluciones Técnicas Implementadas

Durante el desarrollo y despliegue de este proyecto, se resolvieron con éxito los siguientes retos de arquitectura web:

1. **Consumo de API RESTful Dinámica**: Se integró `axios` para conectar la interfaz con el backend remoto en Vercel, capturando los parámetros de la ruta (`route.params.cui`) para renderizar dinámicamente la información del alumno y sus asignaturas matriculadas.
2. **Solución Definitiva a Bloqueos de CORS**: Dado que el backend restringe las peticiones de dominios externos en producción, se implementó un **Proxy Inverso (Túnel de Redirección)** a través de los servidores de Netlify utilizando un archivo de configuración `_redirects`.
3. **Manejo de Single Page Application (SPA) en Servidores Estáticos**: Se configuró el redireccionamiento forzado (`200!`) para asegurar que las rutas del cliente administradas por *Vue Router* carguen directamente y no generen errores `404 Not Found` al recargar la página en el navegador.

---

## 📦 Configuración del Proyecto (Project Setup)

Si deseas clonar y ejecutar este proyecto de forma local en tu computadora, sigue estos pasos:

### 1. Instalar Dependencias
```sh
npm install

This template should help get you started developing with Vue 3 in Vite.

## Recommended IDE Setup

[VS Code](https://code.visualstudio.com/) + [Vue (Official)](https://marketplace.visualstudio.com/items?itemName=Vue.volar) (and disable Vetur).

## Recommended Browser Setup

- Chromium-based browsers (Chrome, Edge, Brave, etc.):
  - [Vue.js devtools](https://chromewebstore.google.com/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd)
  - [Turn on Custom Object Formatter in Chrome DevTools](http://bit.ly/object-formatters)
- Firefox:
  - [Vue.js devtools](https://addons.mozilla.org/en-US/firefox/addon/vue-js-devtools/)
  - [Turn on Custom Object Formatter in Firefox DevTools](https://fxdx.dev/firefox-devtools-custom-object-formatters/)

## Customize configuration

See [Vite Configuration Reference](https://vite.dev/config/).

## Project Setup

```sh
npm install
```

### Compile and Hot-Reload for Development

```sh
npm run dev
```

### Compile and Minify for Production

```sh
npm run build
```
