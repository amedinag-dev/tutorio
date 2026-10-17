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
| `researcher` | Paso 2 (mapa preliminar), paso 3 (investigación completa), huecos que detecte el teacher | tema, slug, modalidad, áreas a cubrir, nivel del perfil, ruta de salida `topics/<slug>/research/`, si el usuario pidió usar el conocimiento del LLM, fecha de hoy |
| `teacher` | Tarea `indice` (4a) al terminar la investigación; tarea `leccion NN` (4b) **solo** cuando el usuario pide la siguiente lección o una concreta; tarea `final` solo cuando el usuario lo pide; refuerzo; alternativas al cambiar el entorno | slug, modalidad, tarea exacta (`indice` \| `leccion NN` \| `final` \| `refuerzo`), rutas de la investigación, resumen del perfil y su entorno |
| `reviewer` | Corregir ejercicios, quiz, refuerzo o final | slug, lección, qué corregir, rutas de respuestas y claves |

- Los subagentes **no ven esta conversación**: cada prompt debe ser autosuficiente
  (rutas exactas, decisiones del usuario, fecha).
- Lánzalos en primer plano cuando el siguiente paso dependa de su resultado.
- Para cursos grandes, divide la investigación y lanza instancias del researcher en
  paralelo por áreas (con rangos de IDs de fuente distintos, p. ej. S100–S199, que
  luego fusionas en `sources.md`).
- **Generación bajo demanda (AGENTS.md regla 10).** Cuando el usuario pida un curso,
  lanza researcher → teacher (`indice`) y **para**: muestra el índice y pregunta con
  qué lección quiere empezar. Nunca lances el teacher con una tarea `leccion` sin una
  petición explícita del usuario en este turno, y nunca le pidas más de una lección
  por petición.
- Tras cada entrega, aplica la checklist de AGENTS.md §8. Comprobaciones útiles con
  `Grep`/`Glob`: afirmaciones sin `[S`, citas `[S<n>]` que no existen en `sources.md`,
  nº de filas de `index.md` igual a la modalidad; tras la investigación, que los
  ficheros de `research/` formen exactamente N grupos `NN` (subficheros `NN.M_*`
  permitidos, ningún número por encima de la modalidad); tras el índice, que no exista
  ninguna carpeta `NN_*`, `.keys/` ni `final/`; tras una lección, que solo haya
  aparecido la carpeta pedida y que tenga `quiz.md`, `exercises/` y `answers/`.

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
