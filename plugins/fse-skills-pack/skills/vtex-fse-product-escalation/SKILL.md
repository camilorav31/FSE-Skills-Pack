---
name: vtex-fse-product-escalation
description: Cómo escribir el ticket de escalación perfecto al equipo de Product Support (PS) de VTEX: qué información incluir sobre el error, cómo documentar la reproducción, y qué sub-equipo de PS corresponde según el módulo afectado. Úsalo cuando el usuario vaya a escalar un caso a producto/PS/engineering y necesite estructurar el contenido completo del ticket (no solo el texto de correo), cuando pregunte qué información le falta antes de escalar, cuando pregunte a qué equipo de PS corresponde un módulo o síntoma específico, o cuando pida revisar si un ticket de escalación ya redactado está completo. Se apoya en vtex-fse-knowledge-base para decidir SI corresponde escalar y confirmar el mapeo de equipos, y en vtex-fse-client-writing para el tono, idioma y formato final del texto; este skill define la ESTRUCTURA y el contenido mínimo que un ticket de escalación debe tener para que PS pueda actuar sin pedir información adicional.
---

# Ticket de escalación perfecto a Product Support (FSE VTEX)

Este skill define qué campos debe tener un ticket de escalación a Product Support (PS) o Engineering para que el equipo destino pueda actuar sin devolverlo pidiendo información básica. Está pensado para cualquier agente del equipo de FSE, no para un caso o persona en particular. No decide el tono ni el idioma del texto (eso vive en `vtex-fse-client-writing`) ni si el caso realmente amerita escalar o a qué sub-equipo pertenece por default (eso vive en `vtex-fse-knowledge-base`); este skill responde "¿qué información tiene que traer el ticket para ser perfecto?".

Usa los tres skills juntos en este orden cuando el agente vaya a escalar un caso real: primero `vtex-fse-knowledge-base` para confirmar que ya se agotó la validación de FSE y para identificar el sub-equipo de PS correcto; luego este skill para asegurar que el contenido del ticket tiene todos los campos necesarios; por último `vtex-fse-client-writing` para el tono, idioma (inglés) y formato de entrega (bloque de código markdown) del texto final.

## Qué hace "perfecto" a un ticket de escalación

Un ticket perfecto es el que PS o Engineering puede leer una sola vez y saber exactamente qué pasó, cómo reproducirlo, y qué se espera de ellos, sin tener que volver a preguntarle al FSE por datos básicos (cuenta, IDs, pasos, evidencia). Cada campo de la estructura de abajo existe para evitar una ronda extra de idas y vueltas.

## Estructura del ticket

Usa siempre código en línea (backticks simples) para cualquier identificador exacto: nombre de cuenta, order ID, SKU ID, transaction ID, request ID, nombre de entidad de Master Data, nombre de app o versión. Esta regla de formato viene de `vtex-fse-client-writing` y aplica también aquí.

**1. Título.** Una línea que resuma cuenta + módulo + síntoma, por ejemplo: "Invoice generation timeout on Order Management for account `ABC` with 50+ SKU orders". Debe permitir identificar el caso sin abrir el ticket.

**2. Cuenta y entorno.** Account name (`accountname`), ambiente si aplica (producción, sandbox), y si el problema es reproducible en más de una cuenta o es específico de esta.

**3. Módulo y equipo destino.** Qué módulo de la plataforma está involucrado (Catalog, Pricing, Checkout, Master Data, Logistics, etc.) y a qué sub-equipo de PS corresponde según la matriz de `vtex-fse-knowledge-base`. Si el caso toca más de un módulo, sépáralo en tickets distintos (ver "un ticket por dominio" en `vtex-fse-knowledge-base`); no mezcles dominios en el mismo ticket para que sea "más eficiente".

**4. Descripción del error.** Qué está fallando, en una o dos frases directas: el mensaje de error exacto si existe (en código en línea), el código de status HTTP si aplica, y en qué punto del flujo ocurre.

**5. Comportamiento esperado vs. observado.** Dos frases separadas: qué debería pasar según la documentación o el diseño esperado de la feature, y qué está pasando en realidad. Esta separación es la que le permite a PS distinguir un bug de una limitación conocida.

**6. Pasos para reproducir.** Una secuencia numerada y verificable: qué cuenta, qué datos, qué acción exacta dispara el error. Debe ser lo bastante concreto para que alguien de PS que no conoce el caso pueda reproducirlo sin preguntar nada más. Si el error depende de una condición específica (ej. "solo con más de 50 SKUs", "solo en checkout con cupón aplicado"), dilo explícitamente como parte de los pasos, no como nota aparte.

**7. Evidencia.** Qué se adjunta o se referencia: HAR file, response de API (con status code y payload relevante), logs de auditoría, screenshots, order IDs o SKU IDs concretos donde se reprodujo. Un ticket sin evidencia adjunta casi nunca es "perfecto", salvo que el síntoma sea tan consistente y simple que no la requiera; en ese caso dilo explícitamente ("no adjunto HAR porque el error es 100% reproducible con estos pasos").

**8. Validaciones ya realizadas.** Qué se descartó antes de escalar y cómo: permisos, configuración de cuenta, error de usuario, feature flags, etc. Esto viene directo de la regla "agotar validación FSE antes de escalar" de `vtex-fse-knowledge-base`; un ticket que no lista esto invita a que PS devuelva el caso pidiendo que se valide lo obvio primero.

**9. Impacto y urgencia.** A quién afecta (un pedido puntual, todos los pedidos de la cuenta, un patrón que probablemente afecte a otras cuentas), y si hay un impacto de negocio concreto (operación bloqueada, pico de ventas, fecha límite). No exageres la urgencia para acelerar respuesta; describe el impacto real.

**10. Workaround.** Si existe un workaround y ya se aplicó o se comunicó al cliente, dilo, y aclara que el escalation sigue en paralelo para la causa raíz. Si no existe workaround, dilo también: eso en sí es información relevante para priorización.

## Matriz módulo → equipo PS

Misma matriz que en `vtex-fse-knowledge-base`; si necesitas actualizarla, actualízala en ambos lugares.

| Dominio / síntoma | Sub-equipo PS |
|---|---|
| Precios, tablas de precio, promociones, cupones | Pricing & Promotions |
| Master Data, sincronización de índices, entidades custom | Storage |
| Productos, SKUs, categorías, marcas, specs | Catalog |
| Envíos, SLAs de entrega, docks, warehouses, Pick and Pack/Last Mile | Logistics |
| Integración con sellers, multivendor | Marketplace |
| Checkout de pago, conectores de pago, transacciones | Payments |
| Bugs de plataforma con evidencia reproducible, limitaciones sin workaround | Engineering |

## Checklist final antes de enviar

Antes de dar el ticket como listo, confirma: que toca un solo dominio/equipo; que incluye pasos de reproducción verificables, no solo una descripción del síntoma; que la evidencia (HAR, logs, IDs) está adjunta o referenciada explícitamente; que las validaciones ya descartadas están documentadas; que no se promete al cliente una fecha ni un desarrollo garantizado solo porque el caso quedó escalado; y que el texto final pasa por `vtex-fse-client-writing` para el tono, el inglés y el formato de bloque de código antes de enviarlo.

## Ejemplo de ticket perfecto

**Input del agente:** "Necesito escalar a producto que la cuenta ABC no puede generar el invoice desde Order Management cuando el pedido tiene más de 50 SKUs, tira timeout. Ya validé que no es tema de permisos ni de configuración de la cuenta. Tengo el HAR file y tres order IDs donde se repite."

**Output (contenido estructurado, antes de pasar por las reglas de tono/formato de `vtex-fse-client-writing`):**

- Título: Invoice generation timeout on Order Management for account `ABC` with 50+ SKU orders
- Cuenta/entorno: `ABC`, producción
- Módulo/equipo: Order Management → Engineering (bug de plataforma con evidencia reproducible)
- Descripción del error: la generación del invoice hace timeout sin devolver respuesta cuando el pedido supera 50 SKUs
- Esperado vs. observado: se espera que el invoice se genere sin importar el número de SKUs del pedido; en la práctica, con más de 50 SKUs la request nunca completa y el Admin muestra timeout
- Pasos para reproducir: 1) en la cuenta `ABC`, crear o localizar un pedido con más de 50 SKUs; 2) ir a Order Management > Invoice; 3) intentar generar el invoice; 4) la request queda cargando hasta hacer timeout
- Evidencia: HAR file adjunto, order IDs `000123`, `000124`, `000125` donde se reprodujo
- Validaciones ya realizadas: se descartó tema de permisos de usuario y de configuración de invoice de la cuenta
- Impacto: bloquea la facturación de todos los pedidos grandes de esta cuenta, no solo uno puntual
- Workaround: no se identificó ninguno; se le indicó al cliente que se está escalando

Este contenido es el que luego se redacta en inglés, con saludo y cierre, dentro de un bloque de código markdown, siguiendo `vtex-fse-client-writing`.
