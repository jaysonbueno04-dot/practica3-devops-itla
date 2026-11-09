# Guía de contribución

## Preparar el entorno

1. Instala Git y Node.js 20 o superior.
2. Clona el repositorio y entra en su carpeta.
3. Ejecuta `npm ci` para instalar las dependencias.

## Cambios y pruebas

- Crea una rama para cada cambio.
- Mantén los cambios pequeños y describe su propósito en el mensaje de commit.
- Añade o actualiza pruebas en `tests/app.test.js` para los cambios de la API.
- Antes de abrir un pull request, ejecuta:

  ```bash
  npm run lint
  npm test
  ```

- Comprueba también que la imagen se construye con `docker build -t tareas-devops .`.

## Pull requests

Abre un pull request hacia `main` o `principal`, explica qué cambió y cómo lo
probaste. El workflow de GitHub Actions valida sintaxis, ejecuta las pruebas y
construye Docker. El despliegue a GitHub Pages se realiza únicamente al hacer
push a la rama principal después de que las validaciones pasen.
