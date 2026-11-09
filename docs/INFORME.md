# Informe de implementación — Práctica 3

## Datos del proyecto

- **Aplicación:** Lista de tareas DevOps — ITLA.
- **Repositorio:** https://github.com/jaysonbueno04-dot/practica3-devops-itla
- **Sitio publicado:** https://jaysonbueno04-dot.github.io/practica3-devops-itla/
- **Tecnologías:** Node.js, Express, Jest, Supertest, Docker y GitHub Actions.

## Implementación

La aplicación ofrece una API Express para crear, listar, completar y eliminar
tareas, además de los endpoints `/health` y `/metrics`. Se añadió
`DELETE /api/tareas/:id`, que devuelve `204` al eliminar una tarea y `404` si
el identificador no existe. El identificador se incrementa de forma monotónica
para evitar colisiones después de eliminar tareas.

GitHub Pages aloja solo archivos estáticos y no ejecuta Express. Por ello, el
frontend detecta Pages y usa `localStorage` para guardar, completar y eliminar
tareas en el navegador; al ejecutar la aplicación con `npm start`, usa la API
Express. Los datos del modo Pages no se sincronizan con el servidor.

Las pruebas de integración usan Jest y Supertest. Cubren la salud de la
aplicación, creación y validación de tareas, cambio de estado, eliminación,
respuestas ante identificadores inexistentes y métricas.

## Pipeline

El workflow `.github/workflows/ci.yml` se dispara con `push` y `pull_request`
en `main` o `principal`. En el job de CI instala dependencias con `npm ci`,
valida sintaxis, ejecuta Jest con cobertura y construye la imagen Docker. Si
esas comprobaciones pasan, un `push` a una de esas ramas publica `public` en
GitHub Pages.

## Verificación y evidencia

- Las verificaciones locales se ejecutan con `npm run lint` y `npm test`.
- Docker se valida con `docker build -t tareas-devops .`.
- La página publicada se abre en el enlace indicado arriba.
- Se observó una ejecución exitosa de GitHub Actions en
  https://github.com/jaysonbueno04-dot/practica3-devops-itla/actions/runs/42538178056
  antes de los cambios documentados en este informe. Después de subir esta
  versión, reemplaza o complementa esa evidencia con el enlace a la ejecución
  exitosa correspondiente a este commit.
- **Demostración rojo → verde pendiente:** para cumplir este punto de la
  práctica, haz temporalmente fallar una expectativa de una prueba, sube el
  cambio y captura el workflow rojo. Luego restaura la expectativa, vuelve a
  subir y captura el workflow verde. No incluyas la expectativa incorrecta en
  la entrega final.

## Conclusión

El pipeline automatiza validaciones y pruebas antes del despliegue y publica
automáticamente el frontend estático en GitHub Pages. La API Express y sus
pruebas se ejecutan localmente y dentro de CI; GitHub Pages sirve únicamente
el contenido estático del directorio `public`.
