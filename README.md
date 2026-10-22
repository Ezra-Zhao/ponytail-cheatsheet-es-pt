# Ponytail en una página · Hoja de trucos de Ponytail (cheatsheet)

[![Stars del proyecto original](https://img.shields.io/github/stars/DietrichGebert/ponytail?style=social)](https://github.com/DietrichGebert/ponytail/stargazers) [![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE) ![Idiomas](https://img.shields.io/badge/idiomas-ES%20·%20PT-blue)

Support & more: [GitHub Sponsors](https://github.com/sponsors/Ezra-Zhao) · [Gumroad — templates & workbooks I use](https://ezraflow28.gumroad.com) · [Ko-fi](https://ko-fi.com/ezra777zhao)

> 🌍 Español (esta página) · [Português](README.pt.md)

**Ponytail** es un conjunto de reglas (un *skill*/prompt) para agentes de programación —Claude Code, Codex, Copilot, Gemini CLI, Cursor y más de 20 agentes, según el proyecto— que obliga al agente a subir una escalera de 7 peldaños antes de escribir código nuevo. La idea en una frase: **el mejor código es el que nunca se escribe**.

Esta hoja resume todo Ponytail en una página: la escalera, los 4 niveles, los comandos y la instalación en 2 pasos. Nota honesta: el repositorio original ya trae su README completo en español; esto no es esa traducción, es la versión de bolsillo en una página, y la versión en portugués no existe en el repo original.

## La escalera: 7 peldaños antes de escribir código

Antes de escribir, el agente se detiene en el **primer peldaño que se sostiene**:

| # | Peldaño | La pregunta que se hace |
|---|---------|--------------------------|
| 1 | **¿Hace falta que exista?** | YAGNI: lo que nadie pidió no se escribe; si se salta algo, se dice en una línea |
| 2 | **¿Ya está en este código?** | Reutiliza el helper, componente o patrón que ya existe, tal como lo usa el código vecino |
| 3 | **¿Lo hace la biblioteca estándar?** | Antes de inventar nada, mira la stdlib de tu lenguaje |
| 4 | **¿Lo trae la plataforma?** | Una API nativa vale más que una librería nueva (ej.: `<input type="date">` en vez de un calendario propio) |
| 5 | **¿Ya tienes la dependencia instalada?** | Úsala; nunca añadas una dependencia por unas pocas líneas |
| 6 | **¿Puede ser una línea?** | Si se entiende de un vistazo, que sea una línea |
| 7 | **Solo entonces: lo mínimo que funciona** | Y si la lógica tiene ramas, bucles, un parser, dinero o seguridad, deja un test pequeño |

Dos reglas que no se negocian:

- La escalera corre **después** de entender el problema —leer el código que toca el cambio y seguir el flujo real—, no en su lugar. Perezoso con la solución, nunca con la lectura.
- **Nunca se recorta**: la validación en los límites de confianza, el manejo de errores que evita pérdida de datos, la seguridad y la accesibilidad.

## Los 4 niveles de intensidad

Cambia el nivel con `/ponytail lite|full|ultra|off`:

| Nivel | Comportamiento | Cuándo usarlo |
|-------|----------------|---------------|
| `lite` | Construye lo pedido, pero nombra en una línea la opción más pequeña y deja que el usuario elija | Cuando quieres mantener el control decisión a decisión |
| `full` | Aplica toda la escalera. Es el nivel **por defecto** | Uso diario |
| `ultra` | Además cuestiona el pedido: antes de construir, discute cualquier parte que la necesidad no justifique | Cuando el código base «te ha ofendido personalmente», en palabras del proyecto |
| `off` | Ponytail desactivado | Cuando necesitas al agente sin filtro |

Opcional: puedes fijar el nivel por defecto con `~/.config/ponytail/config.json` o la variable de entorno `PONYTAIL_DEFAULT_MODE`. Nada es obligatorio.

## Comandos en una línea

| Comando | Qué hace |
|---------|----------|
| `/ponytail-review` | Revisa el diff actual como el dev senior al que llaman cuando algo se rompe: bugs, seguridad, carga real, código arriesgado sin test, partes lentas y lo que sobra. Cada hallazgo dice qué hace el código, qué falla, cómo arreglarlo y qué pasa si no se arregla |
| `/ponytail-audit` | La misma revisión, pero para todo el repositorio y ordenada por prioridad: qué arreglar primero |
| `/ponytail-debt` | Reúne los comentarios `shortcut:` que dejaste para después en un registro, para que «luego» no se convierta en «nunca» |
| `/ponytail-gain` | Muestra el marcador de impacto medido del benchmark del proyecto: menos código, menos coste, más velocidad |
| `/ponytail band` | Solo Claude Code: muestra u oculta la barra sobre el prompt con coste, tokens, tiempo de agente y porcentaje de reutilización. `band import` añade tus sesiones pasadas al tablero local |
| `/ponytail-help` | Referencia rápida de los comandos |

Los comandos necesitan un host con soporte de skills (Claude Code, Codex, Devin CLI, OpenCode, Gemini, Antigravity CLI, pi, Hermes Agent, Qoder, Grok Build). En Codex se invocan con el prefijo del plugin: `$ponytail:ponytail-review`. Los adaptadores de solo instrucciones (archivo de reglas de Cursor, Windsurf, Cline, Copilot, Kiro…) cargan las reglas siempre activas, pero sin los comandos.

## Instalación en 2 pasos

**Claude Code** — dos comandos dentro de la sesión:

```
/plugin marketplace add DietrichGebert/ponytail
/plugin install ponytail@ponytail
```

**Codex** — dos comandos en la terminal:

```bash
codex plugin marketplace add DietrichGebert/ponytail
codex plugin add ponytail@ponytail
```

Después abre `/hooks` en Codex, confía en sus dos hooks de ciclo de vida y empieza un hilo nuevo.

**Cualquier otro agente** (Copilot, Cursor, OpenCode, Gemini CLI y el resto de los 20+): copia el `AGENTS.md` del proyecto original en tu repositorio, o pide a tu agente que instale `skills/ponytail/SKILL.md` como skill. La guía paso a paso por agente está en el `INSTALL.md` del proyecto original.

⚠️ **Instala solo desde la fuente oficial**: `DietrichGebert/ponytail` en GitHub o `@dietrichgebert/ponytail` en npm. El proyecto afirma que nunca distribuye archivos `.exe` ni `.dll`; una copia que los incluya no es la suya.

## El ejemplo que lo explica todo

Petición: «añade un selector de fechas al frontend».

- **Sin Ponytail**: el agente instala una librería de date picker o construye un calendario a mano — **335 líneas**.
- **Con Ponytail**: primero mira lo que ya hay. El repo ya tiene un componente `Input`, y todo navegador trae un selector de fechas. Junta las dos cosas — **10 líneas**.

## Los números (benchmark del propio proyecto)

Cifras de Ponytail 5, medidas por el propio proyecto en Claude Code comparando el mismo agente con y sin el skill (39 tareas, entre ellas un repo real de FastAPI + React, 5 ejecuciones por tarea):

| Métrica | Con Ponytail 5 |
|---------|----------------|
| Código | **−53 %** |
| Tokens de salida | **−45 %** |
| Tiempo | **−41 %** |
| Coste | **−26 %** |
| Lógica arriesgada que sale con test | **98 %** (sin el skill: 68 %) |

Y en las 6 tareas de seguridad del benchmark (inyección SQL, path traversal, tokens falsificados, rate limiting, filas CSV mal formadas, caché) pasó las 30 ejecuciones: menos código, no menos seguro.

> **Nota de honestidad**: son cifras del benchmark publicado por el propio proyecto, no mediciones independientes. La metodología, las tablas por tarea y las limitaciones están en su repositorio (`benchmarks/results/2026-10-07-agentic.md`). Si algo de esta hoja no coincide con la documentación original, vale la original.

## Créditos

- Proyecto original: [DietrichGebert/ponytail](https://github.com/DietrichGebert/ponytail) (licencia MIT) · [ponytail.dev](https://ponytail.dev)
- Esta hoja es un resumen independiente, escrito con palabras propias a partir de la documentación pública del proyecto. No es una traducción oficial ni está afiliada al proyecto.

## 📚 Más hojas de trucos

Otras hojas de trucos del mismo autor, también en español y portugués:

- [Docker 命令速查表 · Docker Cheatsheet](https://github.com/Ezra-Zhao/docker-cheatsheet) — 28 comandos de contenedores
- [FFmpeg 命令速查表](https://github.com/Ezra-Zhao/ffmpeg-cheatsheet) — 28 comandos de audio y vídeo
- [Git 实战速查表](https://github.com/Ezra-Zhao/git-cheatsheet) — 30 comandos diarios y de rescate
- [curl 命令速查表](https://github.com/Ezra-Zhao/curl-cheatsheet) — 29 comandos para depurar APIs
- [正则表达式速查表](https://github.com/Ezra-Zhao/regex-cheatsheet) — 30 patrones prácticos
- [jq 命令速查表](https://github.com/Ezra-Zhao/jq-cheatsheet) — 30 filtros JSON

## Licencia

MIT © 2026 Guangyi Zhao, ver [LICENSE](LICENSE). El proyecto Ponytail también es MIT.
