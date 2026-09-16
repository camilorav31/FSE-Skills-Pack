---
name: vtex-fse-knowledge-base
description: "Base de conocimiento operativa del rol de Field Software Engineer (FSE) de VTEX: qué está dentro y fuera del scope del rol, a qué equipo de Product Support (PS) escalar cada tipo de problema y cómo, el modelo de cobro por créditos de Master Data, y qué herramientas y accesos tiene un FSE (Slack, Grafana, OpenSearch, VTEX CLI, Admin de las tiendas, Health Monitor, Zendesk, VSCode, Postman, Troubleshooter) para saber qué se puede revisar o ejecutar y sugerir validaciones adicionales con esas herramientas. Úsalo cuando el usuario pregunte si algo 'es scope de FSE', cuando dude si debe resolverlo él mismo o escalar, cuando pregunte a qué sub-equipo de PS escalar (Pricing & Promotions, Storage, Catalog, Logistics, Marketplace, Payments, Engineering), cuando pregunte sobre facturación o entidades nativas/no nativas de Master Data, cuando esté evaluando si ya agotó las validaciones antes de escalar, o cuando quiera saber con qué herramienta puede revisar algo (logs, métricas, estado de la orden, etc.). Consúltalo también antes de proponer una solución técnica, para confirmar que no excede el scope de FSE (ej. cambios directos en la cuenta del cliente, scripts, desarrollo custom). No es el skill de redacción: para el texto final de una respuesta o escalation usa vtex-fse-client-writing, y para la estructura del ticket de escalación usa vtex-fse-product-escalation; este skill decide QUÉ hacer, A QUIÉN escalar, y CON QUÉ herramienta validar, no CÓMO redactarlo ni cómo estructurar el ticket."
---

# Base de conocimiento operativa — FSE VTEX

Este skill funciona como manual de referencia rápida sobre el rol de Field Software Engineer (FSE) en VTEX: qué cae dentro de su scope, con qué herramientas puede validar o actuar, y cómo y a quién escalar lo que no. Está escrito para que cualquier agente del equipo de FSE lo use, no solo quien lo redactó originalmente. No cubre reglas de redacción o tono (eso vive en `vtex-fse-client-writing`) ni la estructura detallada de un ticket de escalación (eso vive en `vtex-fse-product-escalation`); este skill responde "¿esto lo resuelvo yo, con qué lo valido, o lo escalo, y a quién?".

Si en algún momento el contenido de este skill entra en conflicto con contexto más reciente que el agente dé en la conversación (una corrección de scope, un cambio de política, una cuenta con reglas especiales), prioriza siempre lo que el agente acaba de decir sobre lo que está escrito aquí, y sugiere actualizar el skill si el cambio parece permanente y aplicable a todo el equipo, no solo a ese caso puntual.

## 1. Rol y funciones del FSE

Un FSE de VTEX opera en un modelo de soporte por niveles. Sus funciones principales son:

- Atender tickets escalados de merchants (cuentas de LATAM y Brasil), típicamente casos que ya pasaron un primer nivel de soporte.
- Diagnosticar la causa raíz de un problema con evidencia técnica (HAR files, respuestas de API, logs de auditoría, exports de datos) antes de comunicar nada al cliente.
- Redactar comunicación clara hacia el cliente y coordinar con los equipos internos de Product Support (PS) cuando el caso lo requiere.
- Mantener documentación viva: una base de conocimiento de infraestructura/arquitectura (hallazgos por cuenta, comportamientos no documentados, casos precedentes) y este tipo de manual operativo.

### Qué SÍ es scope de FSE

- Orientación técnica: explicarle al cliente cómo funciona una feature, dónde configurarla, por qué se comporta de cierta forma.
- Diagnóstico: investigar logs, HAR files, respuestas de API, exports, para identificar la causa raíz de un problema.
- Remediación guiada: decirle al cliente exactamente qué pasos seguir en su propio Admin para resolver o mitigar el problema.
- Validar hipótesis con datos (ej. hacer sampling de API) antes de escalar o antes de confirmarle algo a un cliente.

### Qué NO es scope de FSE

- Ejecutar cambios directamente en el ambiente o la cuenta del cliente. El FSE guía, el cliente ejecuta en su propio Admin. Una excepción a esto requiere autorización explícita de un manager y consentimiento documentado del cliente; nunca asumas que aplica por default. Esto aplica aunque el FSE tenga acceso técnico de sobra para hacerlo él mismo (ver sección 2: tener acceso a una herramienta no equivale a tener autorización para ejecutar el cambio).
- Desarrollo de scripts o cualquier forma de desarrollo custom para el cliente.
- Trabajo de implementación (setup inicial, integración desde cero) que le corresponde a otro equipo (ej. Onboarding, Implementation Partners).
- Decisiones comerciales (descuentos, términos de contrato, SLAs comerciales) o cambios de arquitectura de plataforma que excedan la cuenta puntual.

Si una tarea cae en esta segunda lista, señálalo explícitamente en vez de proponer una solución como si fuera viable dentro del scope: nombra qué equipo o rol sí la cubre si lo sabes, o dilo como pendiente de confirmar si no.

## 2. Herramientas y accesos del FSE

Esta sección existe para que, al diagnosticar o revisar un caso, Claude sepa qué herramientas tiene disponibles el FSE y pueda sugerir activamente una validación adicional con la herramienta correcta, en vez de asumir que la única fuente de información es lo que el agente ya pegó en el chat. Tener acceso a una herramienta es distinto de tener autorización para ejecutar un cambio con ella: el acceso es para diagnosticar y revisar; ejecutar cambios en la cuenta de un cliente sigue las reglas de la sección 1 (qué NO es scope de FSE), sin importar qué tan fácil sea hacerlo técnicamente.

- **Slack**: canal principal de comunicación interna de VTEX. Útil para buscar incidentes conocidos, deploys recientes de producto/engineering, discusión previa sobre una cuenta o módulo, y para coordinarse con otros equipos. Ver `vtex-fse-ticket-reading` para cómo se usa específicamente durante el análisis de un ticket.
- **Claude**: este mismo asistente, usado por el FSE para diagnosticar casos, redactar respuestas y escalations, y correr los demás skills de este pack.
- **Troubleshooter**: webapp interna del equipo para ejecutar rutinas de troubleshooting automatizadas sobre distintas situaciones (por cuenta, por módulo, por tipo de síntoma). Este skill no documenta aún el detalle de qué checks específicos ofrece; si el agente pregunta por una validación puntual que podría existir ahí, busca información al respecto en Slack antes de asumir que no está disponible o de improvisar el detalle.
- **Grafana**: dashboards de métricas e infraestructura. Útil para revisar salud de servicios, tasas de error o latencias alrededor de la fecha en que empezó un incidente, como complemento a lo que se ve en el ticket.
- **OpenSearch**: búsqueda y exploración de logs de la plataforma. Útil para buscar trazas o errores específicos asociados a una orden, request o cuenta cuando el HAR file o el export no son suficientes.
- **VTEX CLI**: herramienta de línea de comandos para interactuar con la plataforma VTEX (workspaces, apps, link, publish, etc.). Útil para reproducir o probar comportamiento en un workspace de forma controlada, sin tocar el ambiente de producción del cliente.
- **Acceso total al Admin de las tiendas**: el FSE puede entrar directamente al Admin de la cuenta del cliente para revisar configuración, datos o comportamiento. Este acceso es para diagnóstico y verificación, no cambia la regla de la sección 1: ejecutar un cambio ahí en nombre del cliente sigue sin ser scope de FSE salvo la excepción ya descrita (autorización de manager + consentimiento del cliente documentado).
- **Health Monitor**: herramienta de monitoreo del estado de salud de una cuenta/infraestructura. Útil para revisar si hay alertas o degradación activa relacionada con el módulo del caso.
- **Zendesk**: sistema de tickets donde vive la conversación con el cliente y donde se pega la respuesta final. Ver `vtex-fse-client-writing` y `vtex-fse-ticket-reading`.
- **VSCode**: editor de código, usado para revisar o probar código/configuración técnica (por ejemplo, en un workspace de VTEX CLI), no para desarrollar soluciones custom para el cliente (eso sigue sin ser scope de FSE).
- **Postman**: cliente de APIs usado para llamar directamente a las APIs de VTEX (consultar órdenes, transacciones, simular carritos, etc.) cuando se necesita más control o detalle que el que da el Copilot de WhatsApp. Ver `fse-whatsapp-copilot`, que cubre la alternativa más rápida para el mismo tipo de consultas.

Cuando el agente esté diagnosticando un caso y una de estas herramientas podría aportar evidencia adicional que aún no se ha revisado (por ejemplo, métricas en Grafana, logs en OpenSearch, o una rutina de Troubleshooter), sugiérelo explícitamente como próximo paso en vez de conformarte con la evidencia que ya se pasó en el chat.

## 3. Cuándo y cómo escalar

### Regla general: agotar validación FSE antes de escalar

Antes de escalar a PS, el FSE debe haber agotado las validaciones que están a su alcance: reproducir o entender el síntoma, revisar logs/HAR/API (con las herramientas de la sección 2), descartar causas obvias (permisos, configuración de cuenta, error de usuario), y formular una hipótesis de causa raíz razonablemente sustentada. El escalation debe documentar explícitamente qué se descartó y por qué, no solo describir el síntoma.

### Workaround primero

Si existe un workaround conocido, aplícalo (o indícaselo al cliente) mientras el escalation a PS sigue su curso en paralelo. No dejar al cliente bloqueado esperando una respuesta de PS si hay una mitigación disponible mientras tanto.

### Un ticket por dominio

Cada sub-equipo de PS recibe un ticket separado cuando el caso toca más de un dominio a la vez; no mezclar, por ejemplo, un problema de Pricing con uno de Logistics en el mismo escalation aunque estén relacionados en el mismo caso de cliente.

### Cuándo escalar a Engineering específicamente

No escalar a Engineering solo por sospecha de bug. Se necesita un caso real y concreto de falla (orderId, SKU, timestamp, payload/respuesta de API) que reproduzca el problema, no solo una descripción teórica. Si la evidencia aún es débil, seguir validando a nivel FSE o PS primero.

### Matriz de escalación por dominio (PS sub-teams)

> Este mapeo debe mantenerse alineado con la asignación real vigente en el equipo; cualquier FSE que note un cambio (un dominio que se reasignó a otro sub-equipo, un sub-equipo nuevo) debe corregir esta tabla para que quede vigente para todo el equipo, no solo para su caso puntual. La misma tabla vive también en `vtex-fse-product-escalation`; si la actualizas aquí, actualízala allá también.

| Dominio / síntoma | Sub-equipo PS | Notas |
|---|---|---|
| Precios, tablas de precio, promociones, cupones | Pricing & Promotions | |
| Master Data, sincronización de índices, entidades custom | Storage | |
| Productos, SKUs, categorías, marcas, specs | Catalog | |
| Envíos, SLAs de entrega, docks, warehouses, Pick and Pack/Last Mile | Logistics | Pick and Pack requiere habilitación previa por VTEX, no es self-service |
| Integración con sellers, multivendor | Marketplace | |
| Checkout de pago, conectores de pago, transacciones | Payments | |
| Bugs de plataforma con evidencia reproducible, limitaciones sin workaround | Engineering | Requiere caso real de falla, no solo hipótesis |

### Registrar una necesidad vs. comprometer desarrollo

Cuando Producto acepta "registrar" una necesidad del cliente, eso no implica compromiso de desarrollo ni fecha. Esta distinción se preserva tanto en las notas internas como en cualquier comunicación al cliente sobre el estado de su solicitud: nunca prometer una fecha o un desarrollo garantizado solo porque quedó registrada la necesidad.

## 4. Master Data — particularidades y modelo de cobro

Master Data tiene un modelo de cobro basado en créditos (proyecto interno "Hydra"), pensado para consumo no nativo de la plataforma. Esto es relevante para diagnosticar reclamos de facturación y para explicarle a un cliente por qué le están cobrando (o no) por cierta entidad.

### Entidades nativas vs. no nativas

Este es el eje que determina si algo se cobra:

- **Entidades nativas**: estructuras de datos ligadas a funcionalidad estándar de VTEX (apps oficiales, features nativas de la plataforma). Estas no se cobran bajo el modelo de créditos de Master Data.
- **Entidades no nativas**: estructuras creadas por el cliente o por un desarrollador para cubrir necesidades custom, aunque el caso de uso sea "commerce-related" o esté cubriendo un gap real del producto nativo. Estas sí generan cobro, porque representan carga de procesamiento adicional que no está atada al modelo de servicio estándar de VTEX. Producto ha sido consistente en este punto incluso cuando el cliente argumenta que la entidad custom existe solo para suplir una limitación de una feature nativa (ej. Catalog): la posición de Producto es que eso no exime el cobro, aunque sí puede derivar en un "customer need" hacia el equipo dueño de esa feature nativa para que la cubra a futuro.
- Zona gris a tener en cuenta: hay casos documentados donde entidades generadas automáticamente por apps oficiales de VTEX (ej. `subscription_metric` de la app de Subscriptions, o entidades de Defense Mode) terminan clasificadas como no nativas y por lo tanto facturables, lo cual el cliente puede cuestionar razonablemente. Si aparece este patrón, no asumas que es un bug de clasificación: repórtalo a Storage/Producto para que lo revisen puntualmente, sin prometerle al cliente que se corregirá.

### Estructura de cobro (créditos y tiers)

- Cada merchant recibe créditos según su tamaño/facturación/GMV, que funcionan como una capa gratuita ("free-tier") de Master Data.
- Solo se cobra por entidades no nativas que excedan ese free-tier; el cobro de entidades nativas está bajo evaluación pero no está activo hoy.
- El cobro se calcula por rango de documentos almacenados (un "documento" es un archivo de hasta 100 KB; un archivo más grande cuenta como múltiples documentos según el número de bloques de 100 KB que ocupe).
- Además del cargo por exceso de documentos, los contratos suelen incluir un crédito equivalente a un porcentaje del monto neto pagado por las plataformas VTEX Commerce y VTEX CX, aplicable contra el cobro de Master Data u otros add-ons elegibles.

**Importante:** los montos exactos (umbrales de documentos, tarifa por bloque adicional, porcentaje de crédito) varían según el contrato de cada cliente y han cambiado entre versiones de contrato (contratos antiguos vs. la cláusula actualizada). No cites cifras específicas a un cliente sin confirmar las condiciones de su contrato puntual; si no tienes esa visibilidad, dilo explícitamente en vez de asumir un valor genérico.

### Cómo el cliente puede ver su propio consumo

Existe un dashboard nativo en el Admin para que el cliente monitoree su consumo sin necesidad de abrir un ticket: `https://{cuenta}.myvtex.com/admin/master-data-usage`. Cubre Master Data v1 y v2, y el conteo se actualiza con una foto semanal (no en tiempo real), así que si el cliente borra documentos y no ve el número bajar de inmediato, es esperado — hay que esperar al siguiente refresh semanal antes de tratarlo como una inconsistencia. Si después de ese ciclo el número sigue sin reflejar la baja, hay un artículo de troubleshooting oficial: `https://help.vtex.com/troubleshooting/master-data-billing-did-not-decrease-after-deleting-a-data-entity`.

### Borrado/purga de documentos

No existe una configuración nativa de expiración o purga automática de datos en Master Data. Si un cliente quiere reducir su conteo de documentos, debe construir su propio script que borre periódicamente los documentos que ya no necesita (esto es desarrollo del lado del cliente, no de FSE — ver sección 1). Al diseñar ese script, ten en cuenta que Master Data es un servicio elástico sin una fórmula fija de escalado, así que el script debe manejar throttling con una estrategia de backoff exponencial en las llamadas de borrado; correr el borrado en paralelo con varios threads no ayuda a ir más rápido, porque todos los threads comparten el mismo límite de tasa.

### Cuándo esto es un tema de FSE vs. comercial

Explicar cómo funciona el modelo de créditos, ayudar a interpretar el dashboard de consumo, o diagnosticar por qué una entidad específica está siendo facturada, cae dentro de scope de FSE. Negociar el monto del cobro, pedir una exención o waiver, o disputar una cláusula contractual de Master Data es una decisión comercial (cuenta/Contract Strategy), no algo que un FSE resuelva o prometa por su cuenta; en esos casos, orienta al cliente hacia su ejecutivo de cuenta en vez de comprometerte a una excepción.

## 5. Relación con otros recursos

- **Redacción del texto final** (respuestas a cliente, escalations a PS): usa el skill `vtex-fse-client-writing`, no este.
- **Estructura y contenido mínimo de un ticket de escalación a PS** (título, pasos para reproducir, evidencia, campos que PS necesita para no pedir información adicional): usa el skill `vtex-fse-product-escalation`, que se apoya en la matriz de la sección 3 de este documento.
- **Análisis de un ticket recién pegado** (contraste con documentación y Slack, incidentes/deploys relacionados, cautela con respuestas previas de IA): usa el skill `vtex-fse-ticket-reading`.
- **Contraste con el catálogo público de known issues de VTEX**: usa el skill `vtex-fse-known-issues`, como fuente de validación adicional junto con Slack.
- **Consultas o acciones sobre datos en vivo de una cuenta por WhatsApp** (como alternativa rápida a Postman): usa el skill `fse-whatsapp-copilot`.
- Si durante un ticket se identifica un patrón recurrente que valdría la pena capturar aquí (ej. un cambio permanente en a qué equipo escalar cierto dominio, una nueva particularidad de Master Data, o una herramienta nueva que el equipo empezó a usar), sugiere actualizar este skill en vez de dejarlo solo como nota puntual, para que el resto del equipo se beneficie del hallazgo.
