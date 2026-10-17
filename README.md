# Tutorio

**Tutorio** es un profesor personal multiagente para aprender ingeniería, software, IA,
cloud, DevOps y temas similares mediante **cursos de 7, 15 o 30 lecciones**.

Le dices qué quieres aprender y Tutorio investiga el tema con fuentes verificables,
diseña un índice de lecciones adaptado a tu nivel y a tu entorno, genera cada lección
cuando la pides y corrige tus ejercicios con evidencia. Funciona con cualquier nivel
de partida y cualquier tema técnico.

> Todo el contenido se basa en fuentes citadas (documentación oficial, RFCs,
> especificaciones, papers). Lo que no se puede verificar se marca como
> `[NO VERIFICADO]` y no se enseña como hecho.

## Cómo funciona

Tutorio se organiza en cuatro roles, cada uno con una responsabilidad única:

| Rol | Qué hace |
|---|---|
| `leader` | Orquesta el curso, habla contigo, toma decisiones y mantiene el estado |
| `researcher` | Investiga el tema y registra cada afirmación con su fuente |
| `teacher` | Diseña el índice y genera las lecciones a partir de la investigación |
| `reviewer` | Corrige ejercicios, tests y el proyecto/examen final con rúbricas explícitas |

El ciclo de un curso es:

1. **Alta**: eliges el tema y la modalidad (7, 15 o 30 lecciones).
2. **Investigación**: el `researcher` documenta el tema con fuentes.
3. **Índice**: el `teacher` propone las lecciones, con conceptos y objetivos.
4. **Lecciones bajo demanda**: cada lección se genera solo cuando la pides, de una
   en una, con teoría, ejercicios en tres niveles y un test de repaso.
5. **Corrección**: el `reviewer` evalúa tus respuestas, calcula la nota y detecta
   conceptos débiles para proponerte refuerzo.
6. **Cierre**: proyecto final (temas prácticos) o examen teórico (temas
   conceptuales), cuando tú lo pidas.

Sin bloqueos: puedes pedir cualquier lección en cualquier orden, saltarla o repetirla.
Las fechas solo sirven de estadística, nunca para penalizar el ritmo.

## Requisitos

- [Claude Code](https://claude.com/claude-code) (CLI, app de escritorio o extensión
  de IDE). Los roles `researcher`, `teacher` y `reviewer` están definidos como
  subagentes en `.claude/agents/`.
- Otras herramientas de agentes (Codex, Cursor…) también pueden usarlo:
  `AGENTS.md` es la fuente de verdad común y, si la herramienta no tiene subagentes,
  un único agente asume los cuatro roles de forma secuencial.
- Para las prácticas, lo que necesite cada curso (Python, Docker, Kubernetes local,
  una cuenta cloud…). Tutorio adapta los labs al entorno que declares.

## Primeros pasos

```bash
git clone <url-del-repositorio> tutorio
cd tutorio
claude
```

Al abrir la sesión, el `leader` arranca solo:

- **Primera vez**: te hace una breve entrevista (onboarding) sobre tu experiencia,
  objetivos, entorno de prácticas y ritmo, y crea tu perfil local.
- **Siguientes veces**: resume el estado de tus cursos y te sugiere por dónde seguir.

A partir de ahí, háblale en lenguaje natural:

| Quieres… | Dile algo como… |
|---|---|
| Empezar un curso | "Quiero aprender Terraform" |
| Ver cómo vas | "¿Cómo voy?" / "¿Cómo voy en Docker?" |
| Pedir una lección | "Dame la siguiente lección" / "Abre la lección 3 de Kubernetes" |
| Corregir tus respuestas | "Corrige lo que he entregado de la lección 4" |
| Repasar lo que fallaste | "Dame más ejercicios de lo que fallé" |
| Hacer el cierre | "Quiero hacer el proyecto final" |
| Actualizar tu perfil | "Ya tengo cuenta de AWS" |
| Pausar o retomar | "Pausa el curso de Go" / "Retomo el curso de Go" |

## Tips

- **Responde en los ficheros `answers/`** de cada lección: los enunciados quedan
  intactos y la corrección se puede repetir. Si prefieres contestar en el chat
  (`1: b, 2: d…`), el `leader` lo transcribe por ti.
- **En los tests**, marca `[X]` en una sola opción por pregunta. Si no lo sabes, marca
  `No lo sé`: cuenta como fallo, pero ayuda a detectar conceptos débiles.
- **Ejercicios de código**: cada `README.md` de nivel 2 y 3 indica el comando exacto
  para ejecutar los tests contra tu carpeta `answers/`. Ejecútalos antes de pedir la
  corrección.
- **¿Prisa?** Puedes pedir que la investigación se haga con el conocimiento del propio
  modelo en lugar de buscar fuentes en la web: el curso se genera mucho más rápido, a
  cambio de no tener citas externas verificadas.
- **Cambia tu entorno cuando quieras**: los labs que necesitan algo que no tienes
  quedan marcados como `⏳ Pendiente de entorno` (con una alternativa local si existe)
  y se revisan al actualizar el perfil.
- **Dominio ≠ bloqueo**: una lección se considera dominada con nota ≥ 80 %, pero es solo
  un indicador. Puedes avanzar igualmente.

## Estructura del repositorio

```
AGENTS.md            # reglas, roles, metodología y formatos (fuente de verdad)
CLAUDE.md            # detalles específicos de Claude Code
.claude/agents/      # subagentes: researcher, teacher, reviewer
templates/           # plantillas de perfil, estado, historial, índice y lección
topics/              # cursos generados (local, no versionado)
progress/            # tu perfil, historial y correcciones (local, no versionado)
```

Solo se versiona la configuración de los agentes. Los cursos (`topics/`) y tu progreso
(`progress/`) se generan en tu máquina y están en `.gitignore`, así que tus datos
personales y respuestas nunca se suben al repositorio.

## Idioma

Todo el material se genera en **español**, manteniendo los términos técnicos clave en
inglés ("un Pod se programa en un Node").

## Licencia

[MIT](LICENSE)
