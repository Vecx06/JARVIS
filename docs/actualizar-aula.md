# Cómo actualizar los datos del Aula de Jarvis

Objetivo: dejar en `aula.json` todas las evaluaciones, entregas y tareas que vienen en el semestre,
sacadas del Aula USM (https://aula.usm.cl), y subir el cambio para que el iPhone de Victor lo reciba.

## Requisitos

- Claude Code con la integración de Chrome activa (`/chrome` debe decir "Status: Enabled").
- Victor con sesión iniciada en el Aula en ese Chrome. Si aparece la página de inicio de sesión,
  detente y pídele que inicie sesión él mismo. Nunca escribas su contraseña.

## Dónde está la información

1. **Calendario del Aula** (quizzes y tareas con hora exacta). Desde una pestaña de `aula.usm.cl`,
   la API interna de Moodle responde a `POST /lib/ajax/service.php?sesskey=<M.cfg.sesskey>` con el método
   `core_calendar_get_calendar_monthly_view` (args: `year`, `month`, `courseid: 1`, `categoryid: 0`,
   `includenavigation: false`, `mini: false`, `day: 1`). Revisa los meses que quedan del semestre.
2. **Páginas de cada ramo**: `/course/view.php?id=<id>&section=<n>`. Pídelas de a una: en paralelo,
   el Aula responde con un error de bloqueo de sesión.
3. **Cronogramas oficiales** (las fechas de certámenes y controles casi siempre están aquí, no en el
   calendario). Ábrelos en el visor de PDF de Chrome y léelos con capturas de pantalla:
   - INF129: página `/mod/page/view.php?id=8130034` (planilla de Google publicada).
   - FIS111: carpeta `/mod/folder/view.php?id=8057339` (Programación de cátedra).
   - MAT061: carpeta `/mod/folder/view.php?id=8093552` (CRONOGRAMA MAT-061).
   - MAT070: recurso `/mod/resource/view.php?id=8161243` (Cronograma MAT070).
   - HRW133: recurso `/mod/resource/view.php?id=8338795` (Calendario actualizado).
   - Revisa también si apareció material nuevo con fechas (distribución de salas, avisos del foro).

Cursos 2026-2: FIS111 62273, EFI201 62206, HRW133 62375, MAT061Y 63432, MAT070T 62575, MAT070 62573,
MAT061L 62572, MAT061 62571, INF129 62813. Si el semestre cambia, búscalos en
`core_course_get_enrolled_courses_by_timeline_classification` con `classification: "inprogress"`.

## Estructura de `aula.json`

```json
{
  "version": "AAAA-MM-DD",
  "generado": "AAAA-MM-DDTHH:MM",
  "fuente": "texto corto",
  "eventos": [
    { "titulo": "Certamen 3 · MAT061 Álgebra Lineal", "fecha": "2026-11-23", "tipo": "evaluacion", "notas": "..." },
    { "titulo": "Tarea 3: Listas · INF129", "inicio": "2026-10-30T17:30", "fin": "2026-10-30T17:30", "tipo": "entrega", "notas": "..." }
  ],
  "tareas": [
    { "titulo": "Entrega T4P1 (40%) · INF129", "fecha": "2026-11-06", "prioridad": "alta", "notas": "..." }
  ],
  "contexto": [ "frases cortas con materia que viene, fin de semestre, etc." ]
}
```

- `eventos`: usa `fecha` (AAAA-MM-DD) si no se sabe la hora, o `inicio`/`fin` (AAAA-MM-DDTHH:MM, hora
  de Chile) si se sabe. `tipo` es `evaluacion` o `entrega`.
- `tareas`: cosas con fecha aproximada (por ejemplo "esta semana, en tu laboratorio").
- Pon la materia que entra en cada evaluación en `notas` cuando el cronograma la indique.
- Solo fechas desde hoy en adelante. No inventes nada: si una fecha no está clara, ponla en `notas`.
- **Cambia `version` a la fecha de hoy.** Si no cambia, la app no recarga los datos.

## Al terminar

1. Valida que el JSON sea correcto (por ejemplo, con `python -m json.tool aula.json`).
2. Muéstrale a Victor qué cambió: evaluaciones nuevas, fechas movidas, cosas que se eliminaron.
3. Con su confirmación: `git add aula.json`, `git commit -m "Actualizar datos del Aula"`, `git push`.
4. Dile que cierre y vuelva a abrir Jarvis en el iPhone para ver los datos nuevos.
