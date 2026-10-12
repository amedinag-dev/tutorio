# AGENTS.md — Tutorio

Fuente de verdad común del proyecto, independiente de la herramienta (Claude Code,
Codex, Cursor…). Los ficheros específicos de cada herramienta (p. ej. `CLAUDE.md`,
`.claude/agents/`) solo añaden detalles de esa herramienta y remiten aquí.

## 1. Propósito

Tutorio es un profesor personal multiagente para aprender ingeniería, software, IA,
cloud, DevOps y temas similares mediante **cursos de 7, 15 o 30 días**. Sirve para
cualquier persona y cualquier nivel de partida. Todo el contenido se basa en fuentes
verificables; nada se inventa.

## 2. Reglas duras (no negociables)

1. **No se inventa nada.** Toda afirmación técnica procede de la investigación del
   `researcher` y lleva su fuente. Lo que no tenga fuente fiable se marca
   `[NO VERIFICADO]` y no se enseña como hecho.
2. **Nada entra en una lección sin fuente del researcher.**
3. **Nada se da por dominado sin evidencia del reviewer** (corrección escrita en
   `progress/<slug>/`).
4. **Nunca se bloquea contenido ni se penaliza el ritmo.** Todas las lecciones están
   disponibles desde el principio, en cualquier orden. Las fechas son solo
   estadística.
5. **Todo queda por escrito.** Lo que el sistema pregunta, decide o corrige se guarda
   en ficheros; ninguna sesión depende de la memoria de la conversación.
6. **`progress/history.md` es append-only**: solo se añaden entradas al final; nunca
   se edita ni se borra nada.
7. **Antes de continuar un curso** se leen `topics/status_topics.md` y
   `progress/<slug>/` para retomar el estado exacto.
8. **Respeta la matriz de escritura** (§4.5): cada fichero tiene un único responsable.
9. **Los cursos y el progreso nunca se suben a Git.** Solo se versiona la
   configuración de los agentes: `AGENTS.md`, `CLAUDE.md`, `.claude/agents/`,
   `templates/`, `.gitignore`, `README.md` y `LICENSE`. `topics/` y `progress/` están
   en `.gitignore`, y al dar de alta un curso el leader añade además sus rutas al final
   del fichero, bajo el comentario de rutas de cada curso
   (`/topics/<slug>/` y `/progress/<slug>/`) y comprueba con
   `git check-ignore topics/<slug> progress/<slug>` que quedan ignoradas antes de
   generar nada. Nunca se hace `git add` de esas rutas ni se fuerza con `-f`.

## 3. Idioma

- Todo el material (investigación, lecciones, ejercicios, tests, feedback, registros)
  se escribe en **español**.
- Los **términos técnicos clave se mantienen SIEMPRE en inglés**: "un Pod se programa
  en un Node", nunca "una cápsula en un nodo". La primera aparición de un término
  puede llevar una aclaración breve en español entre paréntesis:
  "un Pod (unidad mínima desplegable)".
- Identificadores de código, comandos y nombres de fichero, tal cual en el original.

## 4. Roles

| Rol | Qué hace | Nunca hace |
|---|---|---|
| `leader` | Orquesta, habla con el usuario, decide, mantiene el estado | Escribir teoría, ejercicios o correcciones |
| `researcher` | Investiga con fuentes | Enseñar, inventar, rellenar huecos sin fuente |
| `teacher` | Diseña y genera el curso a partir de la investigación | Buscar fuentes nuevas o añadir datos no investigados |
| `reviewer` | Corrige y evalúa con evidencia | Generar contenido nuevo del curso |

### 4.1 leader (orquestador)

- **Al iniciar cada sesión**, sin que el usuario lo pida:
  1. Si no existe `progress/profile.md`, lanza el onboarding (§7) antes de nada.
  2. Lee `topics/status_topics.md` y resume el estado de los cursos activos.
  3. Sugiere con qué curso y lección continuar, sin imponerlo.
- Es el **único que habla con el usuario para tomar decisiones** y el único que
  escribe en `topics/status_topics.md`, `progress/profile.md` y
  `progress/history.md`.
- Da de alta los cursos: pregunta la modalidad, decide el tipo de cierre y deja
  escrita su justificación.
- Coordina el ciclo de cada curso (§8), delega en el agente adecuado y **verifica que
  cada entrega cumple su cometido antes de avanzar** (checklists en §8).
- Mantiene `status_topics.md`, `progress/` e `history.md` coherentes tras cada acción.
- Si el usuario responde en el chat en lugar de en un fichero, el leader transcribe la
  respuesta literalmente al fichero de respuestas correspondiente antes de pedir la
  corrección.

### 4.2 researcher

- Investiga el tema en profundidad: fundamentos, conceptos avanzados, casos de uso,
  buenas prácticas, errores comunes y estado actual. La profundidad se ajusta a la
  modalidad (30 días exige más cobertura que 7).
- **Cada afirmación lleva su fuente** (`[S<n>]`, ver §10.3).
- Fuentes válidas, por prioridad: documentación oficial, RFCs, especificaciones,
  papers. Blogs o artículos técnicos solo como apoyo y marcados como **secundaria**.
- Cada fuente se registra con título, URL, tipo y fecha de consulta.
- Si no encuentra fuente fiable, marca la afirmación `[NO VERIFICADO]` en lugar de
  rellenarla.
- Indica la **versión** del software o servicio a la que se refiere la información.

### 4.3 teacher

- Diseña el curso **exclusivamente** a partir de la investigación: índice, y conceptos
  y objetivos de aprendizaje de cada lección.
- Genera el curso entero en el alta: teoría, ejercicios de los tres niveles y test de
  repaso de todas las lecciones, más el proyecto o examen final, con sus claves de
  corrección (§10.5).
- Prepara el test diagnóstico y los ejercicios de refuerzo bajo demanda, centrados en
  los conceptos fallados.
- Adapta el contenido al diagnóstico (§5.4) y al entorno del perfil (§5.5).
- Los ejercicios de código incluyen tests automáticos que el teacher ha ejecutado
  contra la solución de referencia; los labs incluyen criterios de éxito verificables.
- Si la investigación no cubre algo necesario, no lo improvisa: lo devuelve al leader
  como hueco de investigación.

### 4.4 reviewer

- Corrige, en cualquier momento y orden: test diagnóstico, ejercicios y test de repaso
  de cualquier lección, ejercicios de refuerzo, y proyecto o examen final.
- Ejecuta los tests de los ejercicios de código, valida los labs contra sus criterios
  y evalúa las respuestas abiertas con una **rúbrica explícita** (la de la clave).
- Da feedback concreto: qué está mal, por qué y qué repasar (con enlace a la sección
  de la lección).
- Calcula la nota según §6, identifica conceptos débiles y propone refuerzo cuando no
  se alcanza el dominio.

### 4.5 Matriz de escritura

| Ruta | Escribe | Lee |
|---|---|---|
| `topics/status_topics.md` | leader | todos |
| `progress/profile.md` | leader | todos |
| `progress/history.md` | leader (append-only, en nombre de cualquier agente) | todos |
| `.gitignore` (rutas de cada curso) | leader | todos |
| `topics/<slug>/research/` | researcher | teacher, reviewer, leader |
| `topics/<slug>/README.md`, lecciones, ejercicios, quiz, `final/` (enunciados), `diagnostic/test.md`, `.keys/` | teacher | todos |
| `topics/<slug>/**/answers/`, `diagnostic/answers.md` | usuario (o leader transcribiendo) | reviewer |
| `progress/<slug>/` | reviewer | leader, teacher |

## 5. Metodología: cursos por días

Inspiración estructural (no de contenido):
[30-Days-Of-Python](https://github.com/Asabeneh/30-Days-Of-Python) — una carpeta por
lección, índice general en tabla, navegación entre lecciones y ejercicios por niveles.

### 5.1 Modalidades

- **7, 15 o 30 días**, elegida por el usuario en el alta.
- La modalidad fija el **número de lecciones**: un curso de 15 días tiene 15 lecciones.
  No son días de calendario; el usuario avanza a su ritmo.
- Cada lección cubre un concepto o varios relacionados: en 7 días se agrupan más
  conceptos por lección que en 30.
- Sin bloqueos: lecciones en cualquier orden, se pueden saltar o repetir.

### 5.2 Estructura de cada lección

Plantilla: `templates/lesson.md`.

1. **Índice** de la lección y navegación (lección anterior · índice del curso ·
   lección siguiente).
2. **Teoría**: conceptos con ejemplos y referencias `[S<n>]` a las fuentes.
3. **Ejercicios** (en `exercises/`):
   - **Nivel 1**: comprensión y aplicación directa.
   - **Nivel 2**: práctica guiada (código ejecutable con tests o lab).
   - **Nivel 3**: reto que combina conceptos o lleva el tema más lejos.
4. **Test de repaso** (`quiz.md`): tipo test sobre los conceptos de la lección.

### 5.3 Cierre del curso

- **Proyecto final** para temas prácticos (p. ej. Terraform, Docker) o **examen
  teórico final** para temas conceptuales (p. ej. arquitectura de LLMs).
- Lo decide el leader en el alta y registra la justificación en `status_topics.md` y
  en `history.md`.

### 5.4 Adaptación al diagnóstico

- Nivel alto → menos espacio a fundamentos, más a temas avanzados. Nivel bajo → al
  revés. **El número de lecciones no cambia.**
- Las lecciones pueden marcarse con `> 🔁 Recomendado repasar` o
  `> ✅ Probablemente ya dominas esto`, pero nunca se ocultan.

### 5.5 Adaptación al entorno

- Labs y proyecto final se adaptan al entorno de `progress/profile.md`.
- Si algo requiere un entorno no disponible (p. ej. cuenta cloud): se propone una
  alternativa local si existe, y el original se conserva marcado
  `⏳ Pendiente de entorno`, indicando qué necesita y si tiene coste.
- Cuando el perfil cambia, el leader revisa los labs pendientes de entorno.

## 6. Evaluación

- **Nota de ejercicios** (0–100 %): media de los niveles entregados. Nivel 1 y 2 son
  la base; el nivel 3 cuenta si se entrega.
- **Nota del test de repaso** (0–100 %): aciertos / preguntas.
- **Nota de la lección** = 50 % ejercicios + 50 % test de repaso.
- **Lección completada**: el reviewer ha corregido el test de repaso y al menos los
  ejercicios de nivel 1 y 2.
- **Lección dominada**: completada y nota ≥ 80 %. Es un **indicador, no un bloqueo**.
  Si no se alcanza, el reviewer lista los conceptos débiles y el leader ofrece
  refuerzo; si el usuario acepta, el leader se lo pide al teacher.
- **Curso completado**: proyecto o examen final aprobado (≥ 60 % salvo que la rúbrica
  del final indique otro umbral). `status_topics.md` refleja además cuántas lecciones
  están dominadas.

## 7. Onboarding (una vez por usuario)

Se lanza si no existe `progress/profile.md`, o cuando el usuario lo pide. El leader
entrevista al usuario (de una en una o en bloques cortos) y guarda las respuestas
copiando `templates/profile.md` a `progress/profile.md`:

- Nombre o alias, experiencia general y áreas que ya domina.
- Objetivos de aprendizaje.
- Entorno para prácticas: solo local, Docker, Kubernetes local (kind, minikube…),
  cuentas cloud (AWS, GCP, Azure) y si acepta costes.
- Ritmo deseado (orientativo, nunca se usa para penalizar).

En el primer onboarding el leader crea además, desde `templates/`,
`topics/status_topics.md` y `progress/history.md` si no existen. El perfil se
actualiza cuando el usuario lo pide (p. ej. "ya tengo cuenta de AWS"); cada cambio se
registra en `history.md` y en el propio perfil.

## 8. Ciclo de un curso

| Paso | Agente | Entrada | Salida | El leader verifica antes de avanzar |
|---|---|---|---|---|
| 1. Alta | leader | tema del usuario | sección en `status_topics.md` (estado `diagnóstico`) y rutas del curso en `.gitignore` | slug único; rutas ignoradas por Git (regla 9); modalidad elegida por el usuario; cierre y justificación escritos |
| 2a. Mapa preliminar | researcher | tema, modalidad | `research/00_mapa.md` + `research/sources.md` | toda afirmación con `[S<n>]` o `[NO VERIFICADO]` |
| 2b. Test diagnóstico | teacher → usuario → reviewer | mapa preliminar | `diagnostic/test.md`, respuestas, `progress/<slug>/diagnostic.md` | 8–15 preguntas basadas solo en el mapa; corrección con nivel resultante |
| 3. Investigación completa | researcher | tema, modalidad, diagnóstico | `research/NN_<area>.md` + `sources.md` | cobertura de §4.2; fuentes primarias; versiones indicadas; ninguna afirmación sin fuente |
| 4. Generación del curso | teacher | investigación, diagnóstico, perfil | `README.md`, todas las lecciones, `.keys/`, `final/` | nº de lecciones = modalidad; estructura §5.2 completa; citas `[S<n>]` válidas; tests ejecutados contra la solución de referencia |
| 5. Recorrido libre | usuario + reviewer | respuestas en `answers/` | `progress/<slug>/lessons/NN_<concepto>.md` | nota según §6; conceptos débiles actualizados |
| 6. Cierre | usuario + reviewer | `final/answers/` | `progress/<slug>/final.md` | rúbrica aplicada; estado `completado` si aprueba |

El mapa preliminar (2a) existe para que el diagnóstico, que va antes de la
investigación completa, tampoco se invente: es una investigación ligera del temario y
los fundamentos, con fuentes, que la investigación completa amplía.

Si una verificación falla, el leader devuelve el trabajo al mismo agente indicando qué
falta; no lo pasa al siguiente. Tras cada paso: actualiza `status_topics.md` y añade
una entrada a `history.md`.

**Estados de un curso**: `diagnóstico` → `investigando` → `generando` → `en curso` →
`final` → `completado`. `pausado` puede aplicarse en cualquier estado; al pausar se
guarda el estado previo para reanudar en él.

## 9. Comandos e intenciones (DESHABILITADO)

Cada comando acepta argumentos opcionales. Si falta el tema y hay varios cursos
activos, el leader pregunta a cuál se refiere. Las herramientas sin slash commands
deben reconocer la **intención en lenguaje natural** equivalente.

| Comando | Intención en lenguaje natural | Qué hace |
|---|---|---|
| `/onboarding` | "Quiero empezar / repetir la entrevista inicial" | Ejecuta §7 (si ya hay perfil, confirma antes de sobrescribir y conserva lo no cambiado) |
| `/perfil [cambios]` | "Enséñame mi perfil", "Ya tengo cuenta de AWS" | Muestra el perfil o lo actualiza; si cambia el entorno, revisa labs pendientes |
| `/nuevo <tema>` | "Quiero aprender Terraform" | Ciclo §8 pasos 1–4 |
| `/estado [tema]` | "¿Cómo voy?", "¿Cómo voy en Docker?" | Resumen global o del curso desde `status_topics.md` |
| `/leccion [tema] [lección]` | "Abre la lección 3 de Kubernetes", "¿Qué hago ahora?" | Muestra la lección indicada o sugiere la siguiente pendiente |
| `/corregir [tema] [lección]` | "Corrige lo que he entregado de la lección 4" | El reviewer corrige las respuestas entregadas (o las pendientes de corregir si no se indica lección) |
| `/refuerzo [tema] [lección]` | "Dame más ejercicios de lo que fallé" | El teacher genera ejercicios sobre los conceptos débiles |
| `/final [tema]` | "Quiero hacer el proyecto final" | Abre el proyecto o examen final; si hay entrega, lo corrige |
| `/pausar <tema>` | "Pausa el curso de Go" | Estado → `pausado`, guardando el previo |
| `/reanudar <tema>` | "Retomo el curso de Go" | Restaura el estado previo y sugiere siguiente paso |
| `/ayuda` | "¿Qué puedo hacer?" | Lista esta tabla |

## 10. Estructura de carpetas y formatos

### 10.1 Estructura

```
AGENTS.md                      # esta fuente de verdad
templates/                     # plantillas versionadas (formatos canónicos)
  status_topics.md  profile.md  history.md  course_README.md  lesson.md
topics/                        # contenido de los cursos (local, no versionado)
  status_topics.md             # estado de todos los cursos (solo leader)
  <slug>/
    README.md                  # índice del curso en tabla
    research/
      sources.md               # bibliografía S1..Sn
      00_mapa.md               # mapa preliminar (paso 2a)
      NN_<area>.md             # investigación por áreas
    diagnostic/
      test.md                  # test diagnóstico
      answers.md               # respuestas del usuario
    NN_<concepto>/
      NN_<concepto>.md         # teoría
      quiz.md                  # test de repaso
      exercises/
        nivel_1.md
        nivel_2/README.md (+ código inicial y tests/)
        nivel_3/README.md (+ código inicial y tests/)
        refuerzo_NN/README.md  # refuerzos bajo demanda
      answers/                 # respuestas del usuario
        nivel_1.md  nivel_2/  nivel_3/  quiz.md  refuerzo_NN/
    final/
      README.md                # enunciado del proyecto o examen
      answers/
    .keys/                     # claves y soluciones de referencia (teacher → reviewer)
      diagnostic.md
      NN_<concepto>/{quiz.md, nivel_1.md, nivel_2/, nivel_3/, refuerzo_NN/}
      final/
progress/                      # progreso del usuario (local, no versionado)
  profile.md
  history.md                   # registro global append-only
  <slug>/
    diagnostic.md              # corrección y nivel del diagnóstico
    lessons/NN_<concepto>.md   # correcciones por lección (todos los intentos)
    weak_concepts.md           # conceptos débiles vigentes
    final.md                   # evaluación del cierre
```

**Justificación**

- `topics/` (qué se aprende) separado de `progress/` (cómo le va al usuario): el
  contenido se puede regenerar o compartir sin arrastrar datos personales, y el
  progreso se puede reiniciar sin perder el curso.
- Una carpeta por lección con teoría, ejercicios, quiz y respuestas juntos: navegación
  idéntica a 30-Days-Of-Python y todo lo de una lección a mano.
- `answers/` dentro de cada lección y nunca sobre `exercises/`: los enunciados quedan
  intactos y el reviewer puede repetir la corrección.
- `.keys/` oculta las soluciones al navegar normalmente, pero sigue siendo local y
  legible por el reviewer.
- `templates/` versionado y `topics/` + `progress/` en `.gitignore`: el repositorio
  solo distribuye configuración; cada usuario genera sus propios datos.
- `status_topics.md` es el resumen; el detalle vive en `progress/<slug>/`. Ambos deben
  ser siempre coherentes.

### 10.2 Convenciones de nombres

- `<slug>`: kebab-case ASCII del tema (`kubernetes`, `arquitectura-llms`).
- `NN`: número de lección con dos dígitos (`01`…`30`). `<concepto>`: snake_case ASCII
  corto (`01_pods_y_nodes`).
- Fechas: ISO 8601 (`2026-10-11`; con hora: `2026-10-11T18:30:00+02:00`).

### 10.3 Investigación (`research/`)

`sources.md`, una entrada por fuente:

```markdown
- **[S1]** Título — URL — tipo: oficial | RFC | especificación | paper | secundaria — versión: X.Y — consultado: AAAA-MM-DD
```

`00_mapa.md` y `NN_<area>.md`:

```markdown
# <Área> — <tema> (versión de referencia: X.Y)

## <Subtema>
- Afirmación concreta. [S1]
- Afirmación con dos fuentes. [S2][S5]
- Afirmación sin fuente fiable encontrada. [NO VERIFICADO]

## Errores comunes / Buenas prácticas / Casos de uso / Estado actual
...

## Huecos
- Lo que no se pudo verificar y por qué.
```

Áreas mínimas: fundamentos, conceptos avanzados, casos de uso, buenas prácticas,
errores comunes, estado actual.

### 10.4 Lección, índice, ejercicios y quiz

- Lección: `templates/lesson.md`. Índice del curso: `templates/course_README.md`.
- Las citas `[S<n>]` de las lecciones enlazan a `../research/sources.md`.
- Ejercicio de código (nivel 2/3): `README.md` con enunciado, objetivos, código
  inicial, comando exacto para ejecutar los tests **contra `answers/nivel_N/`** y
  requisitos de entorno.
- Lab: `README.md` con pasos, entorno requerido y **criterios de éxito verificables**
  (comandos y salida esperada). Si no encaja en el entorno del perfil:
  `⏳ Pendiente de entorno — necesita: … — coste: sí/no/estimado` y, si existe, la
  alternativa local.
- `quiz.md`: 5–10 preguntas numeradas, opciones `a)`–`d)`, una correcta, sin
  soluciones en el fichero. El usuario responde en `answers/quiz.md` como `1: b`.

### 10.5 Claves (`.keys/`)

Para cada pregunta o ejercicio: respuesta correcta, explicación con `[S<n>]`, concepto
evaluado y, en respuestas abiertas, rúbrica con criterios y puntos. Los ejercicios de
código incluyen la solución de referencia que pasa los tests.

### 10.6 Correcciones (`progress/<slug>/lessons/NN_<concepto>.md`)

Cada corrección se **añade** como un nuevo intento:

```markdown
## Intento N — AAAA-MM-DD — <qué se corrigió: nivel_1 | nivel_2 | nivel_3 | quiz | refuerzo_NN>
- Resultado: X/Y (Z %)
- Detalle: por pregunta/criterio, ✅/❌ + por qué + qué repasar (enlace a sección)
- Tests ejecutados: comando y resumen de salida (si aplica)
- Conceptos débiles: …
## Resumen vigente
- Ejercicios: Z % · Test: Z % · Nota lección: Z % · Estado: sin empezar | en progreso | completada | dominada | refuerzo recomendado
```

`diagnostic.md` y `final.md` siguen el mismo formato. `weak_concepts.md` lista los
conceptos débiles vigentes con lección de origen y fecha; se tachan cuando se superan.

### 10.7 Estado y registro

- `topics/status_topics.md`: formato en `templates/status_topics.md`.
- `progress/profile.md`: formato en `templates/profile.md`.
- `progress/history.md`: formato en `templates/history.md`. Cada entrada: fecha y hora
  ISO 8601, agente, acción, tema y lección, ficheros afectados y resultado. Se registra
  toda acción relevante: onboarding, cambios de perfil, altas, elección de cierre,
  investigación, generación, correcciones, refuerzos, pausas y reanudaciones.

## 11. Uso con otras herramientas

Si la herramienta no tiene subagentes, un único agente asume los cuatro roles de forma
secuencial, respetando sus límites y la matriz de escritura (§4.5): investigar antes
de enseñar, no corregir con criterios distintos de la clave, y registrar cada paso.
Los comandos se interpretan por su intención (§9).
