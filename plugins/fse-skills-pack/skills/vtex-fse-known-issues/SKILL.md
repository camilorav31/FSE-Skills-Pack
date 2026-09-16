---
name: vtex-fse-known-issues
description: Consulta help.vtex.com/known-issues, el catálogo público de VTEX que centraliza problemas conocidos de la plataforma (tanto resueltos como no resueltos) con su estado y posibles soluciones, como fuente de validación adicional junto con Slack al diagnosticar un ticket. Úsalo durante el análisis de un ticket para revisar si el síntoma reportado ya coincide con un known issue existente, antes de escalar a PS/Engineering o antes de confirmarle al cliente que algo es un caso aislado de su cuenta. No reemplaza la búsqueda en Slack de `vtex-fse-ticket-reading`; son dos fuentes complementarias del mismo paso de validación.
---

# Known Issues de VTEX (FSE)

`help.vtex.com/known-issues` es el catálogo público de VTEX que centraliza los problemas conocidos de la plataforma junto con su estado y posibles soluciones. Sirve como fuente de validación adicional, junto con Slack (ver `vtex-fse-ticket-reading`), para confirmar si el síntoma de un ticket ya es un problema conocido y documentado por VTEX, en vez de tratarlo como un caso aislado de la cuenta del cliente.

## Cómo está organizado

El listado es filtrable y ordenable (por defecto, "recently updated"), y está categorizado por módulo (Payments, CMS, Order Management, Marketplace Out, Pricing & Promotions, Intelligent Search, entre otros). Cada issue en el listado muestra: estado (por ejemplo "Backlog", u otros estados de seguimiento), módulo afectado, título descriptivo, fecha de publicación, fecha de última actualización, un ID único, y un link al detalle completo. El catálogo es grande (miles de issues), así que conviene filtrar por módulo o buscar por palabra clave del síntoma en vez de recorrer el listado completo.

**Importante:** el listado incluye tanto issues resueltos como no resueltos; no asumas que algo listado ahí ya está resuelto solo por aparecer, ni que si un síntoma no aparece, definitivamente no es un problema de plataforma. El catálogo es amplio pero no exhaustivo ni necesariamente actualizado en tiempo real, así que compléméntalo siempre con Slack y con la evidencia técnica propia del caso.

## Cuándo usarlo

- Durante el análisis de un ticket (ver `vtex-fse-ticket-reading`), como paso adicional de validación junto a Slack: antes de escalar a PS o Engineering, o antes de confirmarle al cliente que algo es "un caso puntual de su cuenta", busca si el síntoma ya coincide con un known issue existente.
- Cuando el agente pregunte directamente si un síntoma es un problema conocido de la plataforma.
- Antes de decirle a un cliente que no hay antecedentes de un comportamiento, para no contradecir un known issue ya documentado públicamente.

## Cómo buscar

1. Identifica el módulo afectado (usa la misma clasificación de módulos que la matriz de escalación de `vtex-fse-knowledge-base`: Payments, Catalog, Pricing & Promotions, Order Management, Logistics, Marketplace, etc.) y las palabras clave del síntoma (el mensaje de error exacto, o una descripción corta y específica del comportamiento).
2. Busca en `help.vtex.com/known-issues` filtrando por ese módulo, o busca directamente por las palabras clave del síntoma.
3. Si encuentras un posible match, abre el detalle completo del issue (no te quedes solo con el título del listado) para confirmar que el comportamiento descrito coincide realmente con el del ticket, y anota su estado y si menciona algún workaround.
4. Si no encuentras nada relevante, dilo explícitamente en tu análisis para el agente ("no encontré un known issue que coincida con este síntoma") en vez de omitir el paso; la ausencia de match también es información útil para el diagnóstico.

## Qué hacer con el resultado

- **Si hay un known issue que coincide:** compártelo con el agente como evidencia de que no es un caso aislado de la cuenta; esto refuerza la hipótesis de causa raíz y puede evitar una escalación duplicada a Engineering si el issue ya está en seguimiento por VTEX (ver "un ticket por dominio" y "cuándo escalar a Engineering" en `vtex-fse-knowledge-base`: si ya existe un known issue documentado, probablemente no haga falta abrir un ticket nuevo, sino referenciar el existente en el escalation).
- **Si el known issue tiene un workaround documentado:** ofrécelo como parte de la respuesta al cliente, citando explícitamente que es un comportamiento conocido de la plataforma (ver la regla "workaround primero" en `vtex-fse-knowledge-base`).
- **Si no hay match:** no lo tomes como confirmación de que es un bug nuevo; sigue con el resto de la validación (Slack, documentación oficial, evidencia técnica propia) antes de escalar.
- Nunca le digas a un cliente que algo "es un known issue conocido" sin haber confirmado el match contra el detalle real del issue, no solo contra el título del listado.

## Relación con otros skills

- Se usa junto con la búsqueda en Slack de `vtex-fse-ticket-reading`, como parte del mismo paso de "contrastar con documentación e incidentes conocidos" durante el análisis de un ticket.
- Si el known issue confirma que hace falta escalar, sigue con `vtex-fse-knowledge-base` (a quién escalar) y `vtex-fse-product-escalation` (cómo estructurar el ticket), citando el known issue como evidencia adicional en el campo de evidencia.
- El texto final hacia el cliente o hacia producto sigue las reglas de `vtex-fse-client-writing` (identificadores y IDs en código en línea, link a la fuente oficial, etc.).

## Ejemplo

**Input:** un ticket reporta que las transacciones se quedan atascadas en estado "Approved" aunque el pago ya fue liquidado ("settled") por el adquirente, lo que bloquea la generación del invoice.

**Acción esperada:** buscar en `help.vtex.com/known-issues`, filtrando por módulo Payments, con palabras clave como "transaction stuck approved settled invoicing"; si aparece un issue cuyo detalle coincide con el comportamiento descrito, contárselo al agente como evidencia de causa raíz conocida (con su ID y estado), y si el issue documenta un workaround, ofrecerlo en la respuesta al cliente citando la fuente.
