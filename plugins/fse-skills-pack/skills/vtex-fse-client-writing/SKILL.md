---
name: vtex-fse-client-writing
description: Reglas de escritura para redactar respuestas a clientes y escalations a producto como Field Software Engineer (FSE) de VTEX. Úsalo siempre que el usuario pida redactar, escribir, generar o dar "una respuesta", "el copy", "el mensaje para el cliente", "una respuesta para el ticket de Zendesk", "un escalation", "escalar esto a producto", "un mensaje para el equipo de producto/engineering", o cualquier texto que el agente vaya a copiar y pegar hacia un cliente o hacia otro equipo interno, incluso si no lo pide explícitamente con esas palabras (por ejemplo "dile al cliente que...", "cómo le explico esto al cliente", "redacta esto para Zendesk", "necesito escalar este bug"). También aplica a follow-ups, actualizaciones de estado y cierres de ticket. No aplica cuando el usuario solo está pensando en voz alta, pidiendo un diagnóstico técnico para sí mismo, o conversando internamente sobre el caso sin intención de enviarlo a un cliente o a otro equipo.
---

# Escritura de respuestas a clientes y escalations (FSE VTEX)

Este skill define cómo redactar el texto final que un Field Software Engineer de VTEX envía hacia afuera: a un cliente por Zendesk, o a otro equipo interno (típicamente producto/engineering) como escalation. Está escrito para que cualquier agente del equipo de FSE lo use sin ajustes: no asume un caso, cuenta o persona en particular. El objetivo es que el texto esté listo para copiar y pegar sin necesidad de retocar formato, tono o contenido.

Hay dos tipos de texto que cubre este skill, con reglas distintas: **respuesta a cliente** (sección "Respuestas a cliente") y **escalation a producto** (sección "Escalations a producto"). Determina cuál aplica según a quién va dirigido el texto antes de redactar: si el destinatario es el cliente final, usa las reglas de respuesta a cliente; si el destinatario es un equipo interno de VTEX (producto, engineering, otro nivel de soporte), usa las reglas de escalation. Las reglas de formato (sin bullets, sin "—", bloque de markdown, identificadores en código en línea) aplican a ambos casos salvo que se indique lo contrario.

No aplica a notas internas informales, resúmenes para el propio agente, o análisis técnico que se queda entre el agente y Claude; en esos casos escribe normalmente, sin las restricciones de este skill.

Si hay duda sobre si el texto es para el cliente, para producto, o para uso interno del agente, pregunta antes de redactar.

## Respuestas a cliente

### Tono

La respuesta debe sonar como la de un ingeniero que domina el tema y quiere que el cliente resuelva rápido, no como un documento corporativo ni como un chat informal. Escribe en prosa clara, cercana y profesional: frases completas, sin jerga innecesaria, sin sonar robótico ni excesivamente formal. Ve directo al punto, pero sin sonar brusco: reconoce brevemente lo que el cliente reportó antes de entrar en la solución, y cierra dejando claro el siguiente paso (qué hará VTEX, qué debe hacer el cliente, o que el caso queda resuelto).

Responde en el idioma en que escribió el cliente (o el que indique el agente). Si no se especifica, asume español neutro salvo que el contexto del caso indique otro idioma.

### Restricciones de formato, sin excepciones

Estas reglas existen porque el texto se pega directamente en Zendesk sin edición posterior, así que cualquier desviación llega tal cual al cliente.

**Sin bullet points ni listas numeradas.** El cliente recibe la respuesta como correo o mensaje de ticket, y las listas ahí suelen leerse como plantilla genérica en vez de una explicación pensada para su caso puntual. Si hay varios pasos o puntos, encadénalos en prosa usando conectores naturales ("primero... luego... por último", "además", "en caso de que", "por otro lado"), igual que lo harías explicándolo en una llamada.

**Nunca usar el carácter guion largo "—" (em dash) como separador de ideas.** Reemplázalo por la puntuación tradicional que corresponda según el caso: punto y seguido, coma, punto y coma, o paréntesis. Revisa el texto final buscando específicamente ese carácter antes de entregarlo.

**Usa código en línea (backticks simples) para cualquier identificador técnico.** Nombre de cuenta (account name), SKU ID, order ID, transaction ID, ID de variable, nombre de entidad de Master Data, nombre de app, endpoint, código de cupón, y cualquier otro valor exacto que el cliente o el agente pueda necesitar copiar o buscar. Por ejemplo, en vez de "el cupón SUMMER10 aplica en la cuenta mitienda" escribe "el cupón `SUMMER10` aplica en la cuenta `mitienda`". Esto aplica dentro del bloque de copy (lo que recibe el cliente o el equipo interno) y también fuera del bloque, cuando el agente y Claude están hablando del caso. El objetivo es que el identificador nunca se confunda visualmente con el texto alrededor ni se transcriba con errores. No uses backticks para palabras o conceptos que no son un identificador exacto (no es para dar énfasis).

**Siempre incluir un link a la documentación oficial de VTEX cuando la respuesta describa un comportamiento, configuración, límite o feature de la plataforma.** Usa Help Center, Developer Portal o VTEX Community, en ese orden de preferencia según dónde esté documentado el tema. Si no encuentras una fuente oficial clara para lo que estás afirmando, dilo explícitamente en tu respuesta al agente (fuera del bloque de copy) en vez de inventar o asumir el comportamiento. No agregues links decorativos a temas que la respuesta no menciona.

**Nunca incluir firma ni despedida tipo "Saludos" / "Quedo atento" / nombre al final.** El agente cierra sus tickets con una macro de Zendesk que ya inserta su firma; agregar una firma en el copy produce duplicados o inconsistencias. Termina el texto en la última idea sustantiva o en el siguiente paso, sin cierre de cortesía adicional. Esta restricción es específica de la respuesta al cliente; no aplica a los escalations a producto (ver más abajo).

### Formato de entrega

Cuando el texto sea "la respuesta" para el cliente, entrégalo dentro de un bloque de código markdown (una cerca de tres backticks con la etiqueta `markdown`), así:

<pre>
```markdown
[texto de la respuesta aquí]
```
</pre>

Esto es porque Zendesk (y la mayoría de editores de tickets) rompen los saltos de línea y el espaciado cuando el texto se copia como texto plano desde la conversación; envolverlo en un bloque de código preserva el formato exacto al pegarlo. Todo lo que esté dentro de ese bloque es lo único que el agente copiará: no pongas ahí notas para el agente, aclaraciones, ni los links de referencia si prefieres separarlos (puedes poner los links dentro del bloque si son parte natural de la respuesta al cliente, y comentarios o contexto adicional fuera del bloque, antes o después).

Si el agente pide explícitamente un borrador para revisar juntos antes de tenerlo "listo", o pide variantes, puedes responder en prosa normal fuera del bloque de markdown; la regla del bloque aplica cuando lo que se está generando es el texto final para copiar y pegar.

### Ejemplo

**Input:** "El cliente reporta que no puede aplicar un cupón de descuento en checkout, dice que le sale 'coupon not found'. Ya revisé y el cupón existe pero está fuera de la fecha de vigencia. Redáctame la respuesta."

**Output:**

```markdown
Gracias por la información. Revisamos el cupón `SUMMER10` que mencionas y confirmamos que existe en la tienda, pero actualmente está fuera de su período de vigencia configurado, por lo que el checkout lo rechaza con el mensaje "coupon not found" aunque el cupón exista.

Para que vuelva a funcionar, es necesario ajustar la fecha de vigencia (o crear un nuevo cupón) desde el Admin, en Promociones y Descuentos. Puedes revisar cómo configurar la vigencia de un cupón en la documentación oficial: https://help.vtex.com/es/tutorial/como-criar-um-cupom-de-desconto

Cuando actualices la fecha, el cupón debería aplicarse normalmente en el checkout. Si al intentarlo sigue mostrando el mismo error, cuéntanos y seguimos revisando.
```

Nota cómo no hay bullets, no hay "—", el identificador del cupón va en código en línea, el link es de Help Center, y el texto no cierra con una firma.

## Escalations a producto

Un escalation es el mensaje que el agente envía a otro equipo de VTEX (producto, engineering, u otro nivel de soporte) para pedir ayuda con un caso que excede su alcance como FSE, típicamente un bug, una limitación de la plataforma sin workaround, o una solicitud de intervención directa. El destinatario es interno, no el cliente, así que las reglas cambian en tres puntos concretos frente a una respuesta a cliente.

> Si el agente necesita estructurar el CONTENIDO completo del ticket (título, pasos para reproducir, evidencia, a qué sub-equipo de PS corresponde), usa el skill `vtex-fse-product-escalation`, que define esa estructura en detalle. Este skill (`vtex-fse-client-writing`) cubre el TONO y FORMATO del texto una vez que el contenido ya está decidido.

**Siempre en inglés**, sin importar en qué idioma esté el ticket original del cliente ni el idioma en que el agente escriba la solicitud a Claude. Los equipos de producto de VTEX operan en inglés, así que el escalation se redacta en inglés siempre.

**Siempre abre con un saludo** dirigido al equipo, por ejemplo "Hi team," o "Hi all," seguido del cuerpo del mensaje.

**Siempre cierra con esta despedida exacta**, en su propia línea al final del mensaje: "Best regards, let me know if you need anything else from my side." Esto es lo opuesto a la regla de respuestas a cliente (donde no va firma), porque aquí no hay macro de Zendesk que la agregue.

El resto de reglas de formato se mantiene: sin bullet points (el cuerpo del escalation también va en prosa corrida, aunque puedes usar un párrafo por bloque de información: contexto, comportamiento esperado vs. observado, pasos para reproducir, impacto), sin "—" como separador, identificadores (account name, order ID, SKU ID, etc.) siempre en código en línea, y siempre que cites un comportamiento documentado de la plataforma, incluye el link a la fuente oficial (Help Center, Developer Portal o Community) para que el equipo de producto pueda verificarlo rápido. Igual que con las respuestas a cliente, entrega el texto final dentro de un bloque de código markdown para que el agente lo copie sin perder formato.

### Ejemplo de escalation

**Input:** "Necesito escalar a producto que el cliente ABC no puede generar el invoice desde el Order Management cuando el pedido tiene más de 50 SKUs, tira timeout. Ya validé que no es tema de permisos ni de configuración de la cuenta."

**Output:**

```markdown
Hi team,

I'd like to escalate an issue affecting account `ABC`. When an order contains more than 50 SKUs, generating the invoice from Order Management consistently times out. I already validated that this is not related to account permissions or configuration, so it looks like a platform-level limitation or bug tied to order size.

Could you take a look and let us know if this is a known limitation or something that needs a fix? Happy to share the order IDs and any additional logs you need.

Best regards, let me know if you need anything else from my side.
```

Nota cómo el nombre de la cuenta `ABC` va en código en línea; lo mismo aplicaría a cualquier order ID, SKU ID o entity name que se mencione en el cuerpo.
