# Jarvis: instrucciones para Claude Code

Este repositorio es la app **Jarvis** de Victor: un asistente personal en el iPhone. Es una página web
(PWA) publicada con GitHub Pages en https://vecx06.github.io/JARVIS/. Cada push a `main` la actualiza.

## Qué hay aquí

- `index.html`: toda la app (interfaz, voz, agenda, tareas, memoria, conexión con la API de Claude).
- `aula.json`: evaluaciones, entregas y contexto académico sacados del Aula USM. La app lo carga sola al
  abrirse y reemplaza sus datos del Aula cuando cambia el campo `version`.
- `manifest.json` e `icon-*.png`: instalación en la pantalla de inicio.
- `lib/`: lectores de Word (mammoth) y Excel (SheetJS) que la app carga al adjuntar esos archivos.
- `docs/actualizar-aula.md`: el procedimiento para actualizar `aula.json`.

## Reglas

- Victor es estudiante de primer año; explícale lo que hiciste paso a paso y en español.
- Antes de hacer `git push`, muéstrale un resumen de los cambios y pídele confirmación.
- Nunca escribas claves de API, contraseñas ni datos privados en este repositorio: es público.
- `index.html` es un solo archivo, sin herramientas de compilación. Mantenlo así.
- Si cambias `index.html`, revisa que el JavaScript no tenga errores de sintaxis antes de subirlo.
- Al cambiar `aula.json`, mantén exactamente la estructura descrita en `docs/actualizar-aula.md`.

## Pedidos que llegan desde Jarvis

Jarvis (en el iPhone) puede dejarle a Victor pedidos listos para este computador, por ejemplo
"Actualiza los datos del Aula de Jarvis siguiendo docs/actualizar-aula.md" o "Hazme una guía de estudio
para el Certamen 3 de MAT061". Los documentos que no son parte de la app van fuera de este repositorio,
en la carpeta de trabajo de Victor (por ejemplo `C:\Jarvis\Documentos\<ramo>`), no aquí.
