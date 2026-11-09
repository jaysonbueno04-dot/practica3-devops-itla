# Plantilla DevOps — Electiva 1 (ITLA 2026-C-3)

Aplicación web mínima (lista de tareas) con todo lo necesario para practicar un pipeline **CI/CD** sin cuentas pagas ni Azure:

| Pieza | Archivo | Para qué sirve |
|---|---|---|
| App Node.js/Express | `src/app.js`, `src/server.js` | API de tareas + `/health` y `/metrics` |
| Frontend estático | `public/index.html` | Se publica en GitHub Pages |
| Pruebas automatizadas | `tests/app.test.js` | Jest + Supertest con cobertura |
| Contenedor | `Dockerfile` | Imagen de producción con *healthcheck* |
| Pipeline | `.github/workflows/ci.yml` | Validar sintaxis → pruebas → build Docker → deploy a Pages |

## Requisitos (gratis)
- Cuenta de **GitHub** (usa un repositorio **público**: GitHub Actions es gratuito).
- **Git** y **Node.js 20+**. Opcional para probar el contenedor en tu PC: **Docker Desktop**.

## Ejecutar en tu computadora
```bash
npm install
npm test          # pruebas + cobertura
npm start         # http://localhost:3000
```
Endpoints: `GET /health`, `GET /metrics`, `GET/POST /api/tareas`, `PATCH /api/tareas/:id` y `DELETE /api/tareas/:id`.

Con Docker:
```bash
docker build -t tareas-devops .
docker run -p 3000:3000 tareas-devops
```

## Subirlo a GitHub y activar el pipeline
1. Crea un repositorio **público** vacío en GitHub (sin README).
2. En la carpeta de la plantilla:
   ```bash
   git init
   git add .
   git commit -m "Plantilla inicial"
   git branch -M main
   git remote add origin https://github.com/TU_USUARIO/TU_REPO.git
   git push -u origin main
   ```
3. En GitHub: **Settings → Pages → Source: GitHub Actions**.
4. Abre la pestaña **Actions**: verás el workflow *CI/CD* ejecutándose. Cuando termine, la URL de tu página aparece en el job *Despliegue a GitHub Pages*.

## Pipeline CI/CD

El workflow se activa en cada `push` y `pull_request` dirigido a `main` o `principal`.
El job **Build y pruebas** usa Ubuntu y Node.js 20, instala las dependencias desde
`package-lock.json` con `npm ci`, valida la sintaxis con `npm run lint`, ejecuta
`npm test` con cobertura y construye la imagen Docker. El job **Despliegue a
GitHub Pages** depende de que el anterior termine correctamente y solo se ejecuta
en un `push` a `main` o `principal`; publica el directorio `public`.

Para activar Pages, selecciona **Settings → Pages → Build and deployment →
Source: GitHub Actions**. Consulta los detalles de pruebas y contribución en
[CONTRIBUTING.md](./CONTRIBUTING.md) y el resumen de implementación en
[docs/INFORME.md](./docs/INFORME.md).

En GitHub Pages la API de Express no se ejecuta: el sitio permite agregar,
completar y eliminar tareas usando el almacenamiento local del navegador. Esas
tareas son propias del navegador y no se sincronizan con la API. Para usar la
API compartida por el servidor, ejecuta `npm start` y abre `http://localhost:3000`.

## Evidencia de la demostración

Incluye en la entrega capturas o enlaces a dos ejecuciones de Actions: una
ejecución fallida por una prueba que hayas hecho fallar intencionalmente y la
ejecución posterior exitosa después de corregirla. No dejes la prueba fallida
en el código final.

## Proyecto Final — cómo extenderla
- Persistencia con **SQLite** (o MongoDB) en lugar del arreglo en memoria.
- Publicar la imagen en **GitHub Container Registry** (`ghcr.io`) desde el pipeline.
- Análisis estático real (ESLint) y umbral de cobertura mínima.
- Monitoreo: usar `/metrics` y los logs JSON; definir al menos una **alerta** (ej. errores > 0).
- Documentar: guía de instalación, manual de operaciones y lecciones aprendidas.
