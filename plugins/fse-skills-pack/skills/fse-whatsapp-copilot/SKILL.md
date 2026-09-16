---
name: fse-whatsapp-copilot
description: Explica qué es y para qué sirve el Copilot interno de WhatsApp del equipo de FSE de VTEX (asistente de IA mantenido por el equipo interno de Copilot, con acceso directo a las APIs de VTEX además de capacidades de LLM: puede consultar órdenes, transacciones, productos, simular carritos, modificar orderforms, etc.), cuándo conviene usarlo para complementar el diagnóstico de un ticket que se está analizando en Claude, y qué tipo de instrucciones se le dan. Genera la instrucción final que se pega en WhatsApp dentro de un bloque de código markdown, siempre que el usuario la pida explícitamente. Úsalo cuando el usuario pregunte qué es, para qué sirve o qué alcance tiene el Copilot de WhatsApp, cuando durante el análisis de un ticket haga falta consultar o modificar datos en vivo de una cuenta y valga la pena sugerir usar Copilot en vez de Postman, o cuando el usuario pida explícitamente "la instrucción para Copilot", "el mensaje para el bot de WhatsApp" o algo equivalente.
---

# Copilot de WhatsApp (FSE VTEX)

Este skill explica qué es el Copilot de WhatsApp que usa el equipo de FSE, cuándo conviene recurrir a él durante el análisis de un caso, y cómo debe verse la instrucción que se le manda. Está pensado para cualquier agente del equipo, no para un caso puntual.

## Qué es

El Copilot de WhatsApp es un asistente de IA interno, mantenido por el equipo interno de Copilot (no por el equipo de FSE), al que los FSE acceden por WhatsApp. No es solo un LLM que comparte documentación o pasos de troubleshooting: tiene acceso directo a las APIs de VTEX, por lo que puede ejecutar la mayoría de los métodos que un FSE normalmente correría a mano por Postman, por ejemplo consultar órdenes, consultar transacciones, consultar productos/SKUs, simular carritos, o modificar orderforms.

Es distinto del bot que aparece firmando la primera respuesta automática en los tickets de Zendesk ("Field Software Engineering" / "Miguel Valencia", cubierto en `vtex-fse-ticket-reading`): ese es un bot que le responde directamente al cliente en el ticket; el Copilot de WhatsApp es una herramienta interna que el FSE consulta activamente para su propio trabajo, no algo que hable con el cliente.

## Cuándo usarlo y alcance

Es útil sobre todo como complemento del análisis de un ticket (ver `vtex-fse-ticket-reading`): cuando el diagnóstico requiere datos en vivo de una cuenta que no están en el texto del ticket, como el estado actual de una orden, el detalle de una transacción, la configuración de un producto/SKU, o el resultado de simular un carrito. En esos casos, en vez de sugerir que el agente abra Postman, Claude puede sugerir consultarlo por Copilot de WhatsApp, que simplifica el proceso, y ofrecer redactar la instrucción exacta.

Como el bot puede ejecutar también acciones que modifican datos (por ejemplo, modificar un orderform), ten dos cuidados al sugerir o redactar una instrucción:

- **Identificadores exactos primero.** Nunca sugieras una instrucción con una cuenta, order ID o SKU aproximado o sin confirmar; una instrucción de consulta o modificación con el identificador equivocado actúa sobre la cuenta u orden equivocada. Si falta un identificador exacto, dilo y pídelo antes de generar la instrucción.
- **La capacidad técnica no es lo mismo que el alcance permitido.** Que Copilot pueda ejecutar una acción de escritura (modificar un orderform, por ejemplo) no significa automáticamente que hacerlo caiga dentro del scope de FSE; sigue aplicando lo que dice `vtex-fse-knowledge-base` sobre qué puede ejecutar un FSE directamente y qué requiere que el cliente lo haga en su propio Admin o requiere autorización de un manager. Ante la duda, señala esa distinción al agente en vez de asumir que porque el bot lo permite, es apropiado pedirlo.

## Qué tipo de instrucciones se le dan

Las instrucciones son comandos claros en lenguaje natural, dirigidos directamente a lo que se necesita saber o hacer: consultar el estado de algo, pedir un dato puntual, o simular una acción. Como con cualquier texto operativo de FSE (ver `vtex-fse-client-writing`), cualquier identificador exacto (nombre de cuenta, order ID, SKU ID, transaction ID, ID de orderform) va en código en línea con backticks simples, para que no se confunda con el resto del texto ni se transcriba mal. Algunos ejemplos de instrucciones típicas:

- "Consulta el estado actual de la orden `1234567890-01` en la cuenta `accountname` y dime en qué paso del flujo está y si hay algún error asociado."
- "Simula un carrito con el SKU `12345` para la cuenta `accountname` y dime el precio final aplicado."
- "Revisa el orderform `abc-def-123` de la cuenta `accountname` y dime si algún item tiene un error de disponibilidad o precio."

A diferencia de un escalation a producto (que siempre va en inglés, ver `vtex-fse-client-writing`), la instrucción para Copilot de WhatsApp es para uso interno del propio equipo de FSE, así que se redacta en el idioma en que esté trabajando el agente (español por defecto, salvo que pida otro).

## Formato de entrega

Cuando el usuario pida explícitamente la instrucción para mandarle a Copilot ("dame la instrucción para Copilot", "redacta el mensaje para el bot", o equivalente), entrégala dentro de un bloque de código markdown, igual que en `vtex-fse-client-writing`, para que se copie y pegue en WhatsApp sin perder formato:

<pre>
```markdown
[instrucción para Copilot aquí]
```
</pre>

Todo lo que esté dentro del bloque es lo único que el agente copiará hacia WhatsApp: no pongas ahí aclaraciones ni contexto para el agente: eso va afuera del bloque, antes o después. Si el agente solo está pensando en voz alta sobre si vale la pena consultar Copilot, o pide variantes para elegir, puedes responder en prosa normal sin el bloque; la regla del bloque aplica cuando lo que se genera es el texto final para pegar.

## Ejemplo

**Input:** "El ticket dice que la orden ABC-123 está estancada en PENDING, ayúdame a redactar la instrucción para preguntarle a Copilot por WhatsApp qué está pasando con esa orden."

**Output:**

```markdown
Consulta el estado actual de la orden `ABC-123` y dime en qué paso del flujo está estancada, si hay algún error asociado, y si el estado PENDING corresponde a un problema de pago, de inventario o de otro tipo.
```

## Relación con otros skills

- **Análisis del ticket antes de decidir si hace falta consultar Copilot:** usa `vtex-fse-ticket-reading`.
- **Si la consulta a Copilot confirma que hay que escalar a Product Support:** usa `vtex-fse-knowledge-base` para decidir a quién, y `vtex-fse-product-escalation` para estructurar el ticket.
- **Tono y formato de cualquier texto final hacia el cliente o hacia producto** (no hacia Copilot): usa `vtex-fse-client-writing`.
