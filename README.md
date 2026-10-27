# systematic-debugging

> Ninguna corrección sin causa raíz.

**Depuración sistemática** · Skill para agentes de código de la comunidad [nvvdev](https://github.com/nvv-dev). Funciona en Claude Code como plugin o como skill suelta, y en cualquier agente compatible con el formato abierto [`SKILL.md`](https://agentskills.io).

*English summary at the end.*

## Qué hace

Obliga al agente a construir primero un comando que reproduzca el fallo, leer la evidencia, probar una hipótesis a la vez y corregir donde se origina el valor equivocado. Después de tres intentos fallidos se detiene y reporta lo aprendido, en vez de seguir probando variaciones. Oculta secretos en todo lo que muestra.

## Objetivo

Cortar el ciclo de "parches que ocultan el síntoma" y dejar cada bug con una prueba de regresión.

## Cuándo usarla

- Una prueba falla o aparece un error o stack trace
- Algo funciona en local y no en CI
- Hay un fallo intermitente

## Cuándo no

- Tareas de funcionalidades nuevas sin un fallo de por medio

## Ejemplo

```text
El build de release falla en minify con R8 pero debug compila. Encuentra la causa.
```

## Cómo se activa

Esta skill **se activa sola**: Claude Code lee su descripción y la carga cuando la tarea coincide. También puedes pedirla por nombre ("usa la skill systematic-debugging") o invocarla con `/systematic-debugging:systematic-debugging` (plugin) o `/systematic-debugging` (copiada a `.claude/skills/`).

## Instalación

### Opción 1: Claude Code, como plugin (recomendada)

Dentro de Claude Code:

```text
/plugin marketplace add nvv-dev/claude-plugins
/plugin install systematic-debugging@nvvdev
```

La primera línea agrega el catálogo de nvvdev una sola vez; la segunda instala esta skill. Si prefieres el paquete completo, instala `dev-workflow@nvvdev` en su lugar (incluye esta skill y las demás de [dev-workflow](https://github.com/nvv-dev/dev-workflow)); no instales los dos a la vez.

### Opción 2: Claude Code, copiando la skill

Para un solo proyecto (se comparte con tu equipo por git):

```bash
git clone --depth 1 https://github.com/nvv-dev/systematic-debugging.git /tmp/systematic-debugging
mkdir -p .claude/skills
cp -R /tmp/systematic-debugging/skills/systematic-debugging .claude/skills/
```

Para todos tus proyectos, cópiala en `~/.claude/skills/` en lugar de `.claude/skills/`.

### Opción 3: otros agentes

Copia la carpeta `skills/systematic-debugging/` en la carpeta de skills de tu agente (cada agente documenta la suya). Con la CLI [skills](https://github.com/vercel-labs/skills) de Vercel Labs:

```bash
DISABLE_TELEMETRY=1 npx skills add nvv-dev/systematic-debugging
```

### Verificar

Abre una sesión nueva y pregunta "¿qué skills tienes disponibles?"; `systematic-debugging` debe aparecer. Con el plugin, `/plugin` la muestra como instalada.

### Actualizar o desinstalar

- Plugin: `/plugin marketplace update nvvdev` para actualizar; `/plugin uninstall systematic-debugging@nvvdev` para quitarla.
- Copia manual: repite la copia para actualizar, o borra la carpeta `.claude/skills/systematic-debugging`.

Antes de instalar cualquier skill, incluida esta, conviene leerla: es un solo archivo, [`skills/systematic-debugging/SKILL.md`](skills/systematic-debugging/SKILL.md). Ver [vetting-external-skills](https://github.com/nvv-dev/vetting-external-skills).

## Contenido

| Ruta | Qué es |
|---|---|
| `skills/systematic-debugging/SKILL.md` | La skill: instrucciones en inglés para que los escáneres de seguridad las analicen completas; el agente responde en tu idioma |
| `.claude-plugin/plugin.json` | Manifiesto para instalarla como plugin |
| `.github/workflows/` | Validación de estructura y escaneo con SkillSpector en cada cambio |

## Contribuir

Este repo se **genera** desde [nvv-dev/dev-workflow](https://github.com/nvv-dev/dev-workflow), donde vive la versión canónica. Abre issues y PRs allí. La mejor contribución es un caso real en la sección *Known failures* de la skill, con la regla que lo evita.

## Origen

Escrita desde cero por nvvdev. Inspirada en: [obra/superpowers](https://github.com/obra/superpowers) (systematic-debugging), [mattpocock/skills](https://github.com/mattpocock/skills) (diagnosing-bugs), [garrytan/gstack](https://github.com/garrytan/gstack) (/investigate).

## Licencia

MIT.

---

## English

**systematic-debugging** — Reproduce first, one hypothesis at a time, fix at the source, stop after three failed attempts, redact secrets.

Install in Claude Code with `/plugin marketplace add nvv-dev/claude-plugins` and `/plugin install systematic-debugging@nvvdev`, or copy `skills/systematic-debugging/` into `.claude/skills/` (project) or `~/.claude/skills/` (personal). Other agents: copy the folder into their skills directory, or run `DISABLE_TELEMETRY=1 npx skills add nvv-dev/systematic-debugging`. Loads automatically when the task matches its description. Canonical source: [nvv-dev/dev-workflow](https://github.com/nvv-dev/dev-workflow). License: MIT.
