# FSE Skills Pack

Skills de [Claude Code](https://claude.com/claude-code) para el equipo de **Field Software Engineering (FSE) de VTEX**. Empaquetados como un plugin de Claude Code para que cualquier miembro del equipo los instale con dos comandos, sin copiar archivos a mano ni tocar `~/.claude`.

## Instalación

Dentro de una sesión de Claude Code (`claude`), corre:

```
/plugin marketplace add camilorav31/FSE-Skills-Pack
/plugin install fse-skills-pack@fse-skills-pack
```

Eso es todo. Claude Code descarga el repo, instala los skills y quedan disponibles automáticamente en cualquier conversación (se activan solos según el contexto, no hace falta invocarlos por nombre).

### Actualizar a la última versión

```
/plugin marketplace update fse-skills-pack
/plugin update fse-skills-pack@fse-skills-pack
```

### Desinstalar

```
/plugin uninstall fse-skills-pack@fse-skills-pack
```

## Skills incluidos

| Skill | Para qué sirve |
|---|---|
| `vtex-fse-ticket-reading` | Analiza un ticket pegado en el chat contra documentación oficial y Slack (incidentes, deploys recientes) antes de diagnosticar o responder. |
| `vtex-fse-knowledge-base` | Decide qué es scope de FSE, a qué sub-equipo de Product Support escalar, y con qué herramienta interna validar cada caso. |
| `vtex-fse-known-issues` | Consulta el catálogo público de known issues de VTEX como validación adicional antes de escalar o responder. |
| `vtex-fse-product-escalation` | Estructura el contenido completo de un ticket de escalación a Product Support para que puedan actuar sin pedir más información. |
| `vtex-fse-client-writing` | Reglas de tono y formato para redactar la respuesta final a un cliente o el mensaje de escalation a producto. |
| `fse-whatsapp-copilot` | Explica el Copilot interno de WhatsApp del equipo y genera la instrucción para consultarlo/operarlo durante el diagnóstico de un ticket. |

## Seguridad y mantenimiento

- Este repo es **público**, pero no contiene datos de clientes, credenciales ni URLs internas — solo reglas de proceso y redacción. Antes de agregar un skill nuevo o editar uno existente, revisa que no incluya datos sensibles de una cuenta real.
- Los cambios se distribuyen con un simple `git push` a `main`: cualquiera con acceso de escritura al repo puede publicar una nueva versión que el equipo recibe con `/plugin marketplace update`. Si el equipo crece, considera mover el repo a la organización de GitHub de VTEX y restringir quién puede hacer push a `main` (branch protection + revisión por PR) en vez de dejarlo en una cuenta personal.
- Para editar un skill: modifica su `SKILL.md` en `plugins/fse-skills-pack/skills/<skill>/`, sube el cambio con un mensaje claro y, si aplica, sube el número de `version` en `plugins/fse-skills-pack/.claude-plugin/plugin.json`.

## Estructura del repo

```
.claude-plugin/marketplace.json      # catálogo del marketplace (lo que agrega /plugin marketplace add)
plugins/fse-skills-pack/
  .claude-plugin/plugin.json         # manifiesto del plugin
  skills/<nombre-del-skill>/SKILL.md # cada skill
```
