---
name: vtex-fse-ticket-reading
description: Analiza un ticket pegado en el chat, sin que el agente lo tenga que pedir explícitamente: lo contrasta con documentación oficial de VTEX y con lo disponible en Slack (incidentes conocidos, deploys recientes de producto/engineering en los módulos involucrados, en una ventana de aproximadamente un mes), y trata con cautela cualquier primera respuesta que el ticket ya tenga firmada por "Field Software Engineering" o "Miguel Valencia", que es la respuesta automática generada por IA/Copilot y puede estar equivocada, incompleta o referenciar módulos/opciones deprecadas. Úsalo siempre que el usuario pegue el texto o el contenido de un ticket (Zendesk u otro sistema) para analizarlo, diagnosticarlo, decidir cómo responder, o antes de escalar. Es el paso de ANÁLISIS previo; no reemplaza a `vtex-fse-knowledge-base` (decide scope y a quién escalar), `vtex-fse-client-writing` (tono/formato del texto final) ni `vtex-fse-product-escalation` (estructura del ticket de escalación).
---

# Lectura y análisis de tickets (FSE VTEX)

Este skill define qué análisis correr sobre un ticket ANTES de diagnosticar, redactar una respuesta, o decidir si se escala. Está pensado para que cualquier agente del equipo de FSE lo aplique de forma consistente, no solo cuando se le pide explícitamente "analiza este ticket": si el agente pega el contenido de un ticket y pide directamente "redacta la respuesta" o "dime si esto es scope", corre este análisis primero, en silencio, como paso previo, y solo después aplica el resultado al resto de la tarea (usando `vtex-fse-knowledge-base` para scope/escalación y `vtex-fse-client-writing` para el texto final).

Si el ticket es tan simple y sin ambigüedad que claramente no amerita buscar incidentes o deploys (ej. una pregunta de configuración básica sin síntoma de bug), puedes saltar los pasos de búsqueda en Slack y decirlo explícitamente en vez de buscar por buscar; el objetivo es evitar diagnosticar a ciegas en casos ambiguos o con evidencia de bug, no forzar una búsqueda en cada ticket trivial.

## Paso 1: Extraer los hechos clave del ticket

Antes de analizar nada, identifica: cuenta afectada, módulo o feature involucrada, síntoma exacto (mensaje de error, comportamiento observado), fechas relevantes (cuándo empezó el problema, cuándo se abrió el ticket), e IDs concretos (order ID, SKU ID, transaction ID) si los hay. Identifica también si el ticket ya tiene una o más respuestas previas dentro del hilo y quién o qué las firmó.

## Paso 2: Identificar si ya hubo una primera respuesta de IA (Copilot)

Las respuestas dentro del ticket firmadas como **"Field Software Engineering"** o **"Miguel Valencia"** son la primera respuesta automática que el sistema de IA/Copilot le da al cliente antes de que un FSE humano intervenga. Trata esa respuesta con las siguientes reglas:

- **No es verdad absoluta.** Puede estar mal diagnósticada, incompleta, o referenciar módulos, opciones o flujos de Admin que ya están deprecados o cambiaron. Nunca la copies o la confirmes al cliente sin verificarla contra la documentación oficial vigente y, si aplica, contra el comportamiento real observado en logs/API.
- **Si al verificarla resulta incorrecta o desactualizada:** dilo explícitamente en tu análisis para el agente (no lo asumas en silencio), y señala que probablemente haga falta corregir o retractar ese mensaje anterior frente al cliente. La corrección al cliente se redacta luego siguiendo `vtex-fse-client-writing` (tono, sin bullets, etc.), pero reconociendo dentro del texto que la información anterior no aplica o quedó desactualizada, sin sonar defensivo.
- **Si al verificarla resulta correcta y pertinente, y el cliente igual escribió de nuevo ignorándola** (por ejemplo, repitiendo la misma pregunta o describiendo el mismo problema sin mencionar que ya lo intentó): señálalo en tu análisis, y al redactar la respuesta considera reforzar explícitamente que la respuesta de Copilot ya contenía la solución correcta, para que el cliente aprenda a leerla con más atención en el futuro (esto es una decisión de tono, no una regla de formato rígida; no lo hagas sonar como un reclamo hacia el cliente).
- Si el ticket no tiene ninguna respuesta previa de "Field Software Engineering"/"Miguel Valencia", omite este paso y continúa.

## Paso 3: Contrastar con documentación oficial y con el catálogo de known issues

Busca en Help Center, Developer Portal o VTEX Community el comportamiento, límite o configuración que el ticket describe (y el que describió, si existe, la respuesta previa de Copilot). Señala explícitamente cualquier discrepancia: una feature que el ticket o la respuesta previa menciona pero que ya no existe o cambió de nombre/flujo, un límite que cambió, o un comportamiento que la documentación actual contradice. Si no encuentras una fuente oficial clara, dilo en vez de asumir.

Complementa esto revisando el catálogo público de known issues de VTEX: usa el skill `vtex-fse-known-issues` para buscar si el síntoma ya coincide con un problema conocido documentado (resuelto o no), lo cual puede reforzar o descartar hipótesis de causa raíz.

## Paso 4: Buscar contexto en Slack

Usa las herramientas de búsqueda de Slack disponibles en la sesión para buscar contexto que no esté en la documentación pública:

- **Incidentes relacionados:** busca menciones del módulo o síntoma en canales de incidentes o de discusión del equipo, para ver si es un problema conocido, ya reportado, o ya tiene una causa raíz identificada por otro FSE o por PS.
- **Deploys o releases recientes:** busca si producto o engineering desplegaron cambios en el módulo afectado en una ventana de aproximadamente un mes antes de la fecha en que empezó el síntoma reportado. Un deploy reciente en el mismo módulo es una hipótesis de causa raíz fuerte que vale la pena mencionar aunque no sea definitiva.
- **Discusión previa de la misma cuenta:** busca si esta cuenta específica ya fue mencionada antes por un tema relacionado, para no repetir diagnósticos ya descartados o para heredar contexto útil (una particularidad conocida de esa cuenta, por ejemplo).

Si las búsquedas no arrojan nada relevante, dilo explícitamente ("no encontré incidentes ni deploys relacionados en Slack en el último mes") en vez de omitir el paso silenciosamente; esa ausencia de evidencia también es información útil para el diagnóstico.

## Paso 5: Síntesis para el agente

Antes de pasar a redactar cualquier texto para el cliente o para producto, resume para el agente (no para el cliente) en pocas líneas: qué está confirmado, qué sigue siendo hipótesis, si la respuesta previa de Copilot era correcta o no y qué implica eso para la respuesta a dar, y qué encontraste (o no) en Slack o en el catálogo de known issues sobre incidentes o deploys relacionados. A partir de esta síntesis, continúa con `vtex-fse-knowledge-base` si hay duda de scope o de a quién escalar, con `vtex-fse-product-escalation` si el caso se va a escalar, y con `vtex-fse-client-writing` para el tono y formato del texto final.

## Ejemplo

**Input:** el agente pega un ticket donde el cliente reporta que no puede generar un cupón con descuento por categoría, y el ticket ya tiene una respuesta previa firmada "Field Software Engineering" que le dice al cliente que use el flujo antiguo de "Promotions Classic" para configurarlo, pero el cliente responde que no encuentra esa opción en su Admin.

**Análisis esperado:** identificar que la respuesta previa de Copilot referencia "Promotions Classic", verificar en la documentación oficial si ese flujo sigue vigente o fue reemplazado (por ejemplo, por el motor de Promotions actual), y si resultó estar desactualizado, señalarlo explícitamente para el agente: la respuesta anterior está obsoleta y hay que corregirla, indicando el flujo vigente. Además, buscar en Slack si hay reportes recientes de otros clientes con la misma confusión tras un cambio de UI en Promotions, lo cual reforzaría que el problema es la documentación/respuesta desactualizada y no un bug puntual de la cuenta.
