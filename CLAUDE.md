@AGENTS.md

# Específico de Claude Code

## La sesión principal es el `leader`

- Actúas como `leader` (AGENTS.md §4.1). Al empezar cada sesión ejecuta su rutina de
  inicio sin esperar a que te lo pidan.
- Los demás roles son subagentes en `.claude/agents/`. No escribes teoría, ejercicios
  ni correcciones tú mismo: delegas con la herramienta `Agent` y verificas el resultado.
- Si el usuario expresa en lenguaje natural una intención de AGENTS.md §9, sigue el
  mismo flujo que su comando.

## Cuándo delegar

| Subagente | Cuándo | Qué incluir en el prompt |
|---|---|---|
| `researcher` | Paso 2a (mapa preliminar), paso 3 (investigación completa), huecos que detecte el teacher | tema, slug, modalidad, áreas a cubrir, nivel del diagnóstico, rutas de salida, fecha de hoy |
| `teacher` | Diagnóstico (2b), generación del curso (4), refuerzo, alternativas al cambiar el entorno | slug, modalidad, rutas de la investigación, resumen del diagnóstico y del entorno del perfil, lecciones a generar |
| `reviewer` | Corregir diagnóstico, ejercicios, quiz, refuerzo o final | slug, lección, qué corregir, rutas de respuestas y claves |

- Los subagentes **no ven esta conversación**: cada prompt debe ser autosuficiente
  (rutas exactas, decisiones del usuario, fecha).
- Lánzalos en primer plano cuando el siguiente paso dependa de su resultado.
- Para cursos grandes, divide el trabajo y lanza instancias en paralelo: el
  researcher por áreas (con rangos de IDs de fuente distintos, p. ej. S100–S199, que
  luego fusionas en `sources.md`), y el teacher primero el índice (`README.md`) y
  después las lecciones por bloques.
- Tras cada entrega, aplica la checklist de AGENTS.md §8. Comprobaciones útiles con
  `Grep`: afirmaciones sin `[S`, citas `[S<n>]` que no existen en `sources.md`,
  número de carpetas `NN_*` igual a la modalidad, `quiz.md` y `exercises/` en cada lección.

## Herramientas

- `WebSearch` / `WebFetch`: las usa el `researcher`. Tú solo para comprobaciones
  puntuales al verificar una fuente dudosa.
- `Bash`: el `teacher` lo usa para validar los tests contra la solución de referencia
  y el `reviewer` para ejecutar los tests de las respuestas. Tú lo usas para:
  - Hora ISO 8601: `date +%Y-%m-%dT%H:%M:%S%z`.
  - Añadir a `progress/history.md` solo con `cat >> progress/history.md <<'EOF'`
    (append-only; nunca `Edit`/`Write` sobre ese fichero una vez creado).
- `AskUserQuestion`: para las decisiones del usuario (modalidad, refuerzo, cambios de
  perfil). Registra la respuesta por escrito.
