---
name: vtex-fse-troubleshooter
description: Catálogo de las herramientas del Troubleshooter de VTEX (troubleshooter.vtex.com), la webapp interna del equipo FSE para investigar y resolver problemas de cuentas sin salir a Postman, Admin o scripts. Úsalo durante el análisis de un ticket para decidir si alguna herramienta del Troubleshooter puede aportar evidencia o resolver el caso (OrderForm, pedidos/OMS, catálogo, Delivery Promise, simulación de fulfillment, pagos/Known Issues de transacciones, BIN, CDN/DNS/certificados, Intelligent Search, sitemap, apps instaladas, Identity, Master Data, timezone, geocoordenadas, order ID, traducciones, etc.) y redirigir al agente a usarla, indicando qué datos debe tener a la mano. Úsalo también antes de escalar a Product Support, para confirmar si la herramienta ya cubre el caso (Timezone, Geocoordenadas y Master Data evitan escalaciones). Distingue herramientas de solo lectura de las que modifican configuración. No reemplaza `vtex-fse-knowledge-base` (qué es scope y a quién escalar) ni `vtex-fse-ticket-reading` (análisis del ticket); complementa ambos con el "con qué herramienta validar".
---

# Troubleshooter VTEX (FSE)

**URL:** https://troubleshooter.vtex.com/home · **Versión documentada:** v3.0.0 · **Idioma de la UI:** portugués

Herramienta interna para investigar y resolver problemas de cuentas VTEX sin salir a Postman, Admin o scripts. Casi todas las herramientas piden **nombre de la cuenta** y siguen un flujo por pasos: Consulta → (Confirmación) → Resultado. Este skill existe para que Claude sepa qué ofrece y pueda **redirigir al agente a la herramienta correcta** cuando el análisis del caso lo amerite. Claude no tiene acceso a la webapp: su rol es indicar cuál herramienta usar, qué datos ingresar y cómo interpretar lo que el agente pegue de vuelta.

## Reglas de uso

1. **Sugiere, no asumas.** Recomienda la herramienta como próximo paso concreto ("Corre X en el Troubleshooter con cuenta + Y") solo cuando el síntoma del ticket coincide con lo que la herramienta revisa. No la sugieras por reflejo en cada ticket.
2. **Prefiere las de solo lectura para diagnosticar.** Son seguras de sugerir sin más trámite.
3. **Las que modifican configuración requieren cuidado.** Antes de recomendarlas: confirma que el diagnóstico las justifica, avisa explícitamente el alcance (cuenta completa, global, irreversible) y recuerda que piden confirmación/checkbox de responsabilidad. Tener acceso a la herramienta no equivale a autorización: aplican las reglas de scope de `vtex-fse-knowledge-base` (qué NO es scope de FSE y cuándo se requiere autorización de un manager y consentimiento documentado del cliente si el cambio se hace sobre la cuenta del cliente). Ante la duda, sugiere el diagnóstico de solo lectura y deja la ejecución del cambio como decisión del agente.
4. **Antes de escalar a Product Support, revisa si el Troubleshooter ya cubre el caso.** Timezone, Geocoordenadas y Validación de Cluster (Master Data) existen justamente para evitar escalaciones.
5. **Cita la evidencia.** Si el agente ejecutó una herramienta, usa el resultado (IDs, flags, request/response) como evidencia en el análisis y en el escalation (ver `vtex-fse-product-escalation`).
6. **Datos de clientes:** la información que devuelven estas herramientas puede ser sensible; en textos hacia el cliente o hacia producto sigue `vtex-fse-client-writing`.

## Home

- **Incidentes abiertos (P0, P1, P2):** carrusel con ID (inc-XXXX), severidad, fecha/hora y, cuando aplica, el Product Support Team responsable. Cada incidente enlaza al canal de Slack. Útil para descartar rápido que el síntoma sea un incidente en curso (complementa la búsqueda en Slack de `vtex-fse-ticket-reading`).
- **Links externos:** Health Monitor, Monitor Center y Status VTEX.
- **Enviar feedback:** botón que lleva a un canal de Slack.

## Herramientas de solo lectura / diagnóstico

| Herramienta (módulo) | Ruta | Qué revisa | Datos que pide | Sugerirla cuando |
|---|---|---|---|---|
| **OrderForm** (Checkout) | /orderform | Extrae toda la información de un orderForm en una sola consulta | Cuenta + ID del orderForm | Problemas de checkout, carrito, precios/promos/envío en el carrito |
| **Segurança** (Seguridad) | /security | Flags de seguridad configuradas en el orderForm de la tienda | Cuenta | Bloqueos de checkout, reCAPTCHA, tokens, comportamiento de seguridad inesperado |
| **Apps instalados** (Apps) | /apps/installedapps | Lista las apps instaladas en un workspace; permite descargar versiones desde master | Cuenta + workspace | Sospecha de app faltante, versión distinta entre workspaces o conflicto de apps |
| **Resumo do pedido** (Pedidos) | /orders/order-summary | Datos de un pedido en el OMS | Cuenta + ID del pedido en el marketplace | Pedido atascado, estado inconsistente, dudas sobre el contenido del pedido |
| **Simulação de fulfillment** | /fulfillment-simulation | Simula selección de sellers, asignación y flete, basado en el checkout debug | Modo OrderForm (ID) o modo Datos de logística (SKUs con cantidad, país, CEP, política comercial) | Seller/flete/SLA inesperado, "por qué se asignó este seller" |
| **Delivery Promise** | /delivery-promise | Explica por qué un SKU aparece o no disponible en Delivery Promise + Shipping, Intelligent Search y Checkout; muestra request/response de cada API | SKU ID, país, CEP y sales channel; hasta **20 líneas** por ejecución (manual o CSV); opción "Usar dpPreview" | SKU no disponible / sin promesa de entrega para un CEP |
| **Catálogo** | /catalog | Analiza un producto: activación, indexación en políticas comerciales, ofertas en caché y datos esenciales de SKUs | SKU ID, Product ID, EAN o RefID | Producto que no aparece o no se puede vender; inconsistencias de catálogo |
| **Consulta BIN** (Pagamentos) | /payments/bin-lookup | Tres modos: BIN específico (si uno de 8 dígitos no está registrado, puede devolver el de 6), información agregada de BINs (API VTEX) y filtrado de BINs por JSON (bandera, nivel, país, emisor…) | BIN o filtro | Rechazos/comportamiento de pago dependiente de bandera, emisor o país de la tarjeta |
| **CDN** | /cdn | Diagnóstico de DNS, CDN y certificados; centraliza dominio, certificado, CDN e infraestructura | URL de producción del dominio | Errores de dominio, certificado SSL, DNS, propagación, comportamiento de caché/CDN |
| **Intelligent Search** | /intelligent-search | Principales configuraciones de búsqueda, indexación y relevancia de la tienda | Cuenta | Búsqueda con resultados inesperados, facets, indexación |
| **Sitemap** | /sitemap | Diagnostica por qué una ruta falta en el sitemap | Cuenta + workspace + path | Página que no aparece en el sitemap / problemas de SEO por sitemap |
| **Validação de Cluster** (Storage, solo la verificación) | /storage/masterdata | Estado del cluster de almacenamiento. Impacta Site Editor, carga de imágenes, FastStore y búsqueda de documentos | Cuenta | Fallas de Site Editor, subida de imágenes, FastStore o documentos de Master Data |

## Herramientas que modifican configuración

Confirmar antes de ejecutar (ver regla 3). Todas piden confirmación; las de impacto amplio exigen checkbox de responsabilidad.

| Herramienta (módulo) | Ruta | Qué hace | Alcance / advertencia |
|---|---|---|---|
| **Block reCAPTCHA v2** (Checkout) | /checkout/block-recaptcha-v2 | Verifica reCAPTCHA v3 y Checkout v6 en todos los sitios antes de bloquear tokens v2 en la config del orderForm. Flujo: pre-checagem → confirmar activación → conclusión | **Afecta a toda la cuenta** |
| **Workspace** (Apps) | /apps/workspace | Crea un entorno limpio en un workspace de desarrollo: remueve apps de terceros e instala un tema estándar de VTEX IO, para aislar roturas de front-end | **Irreversible**; pide URL de producción y scope del app |
| **Geocoordenadas** (Logística) | /logistics | Actualiza latitud/longitud de un código postal | **Aplica a ese CP en toda VTEX (todas las cuentas)**; pide país + CP + ID del ticket con cliente. Para cambiar la dirección completa se abre ticket a Product Support |
| **Timezone da conta** (Logística) | /logistics/account-timezone | Consulta y actualiza país y timezone de una cuenta sin escalar a Producto | Cambia config de la cuenta |
| **Gerenciar Order ID** (Pedidos) | /orders/change-order-id | Consulta y modifica prefijo, sufijo y secuencia del orderId. Flujo: consultar → alterar → resultado | Afecta la generación de futuros pedidos |
| **Domínio Raiz do Auth Cookie** (Identity) | /identity/set-auth-cookie-root-domain | Define el dominio raíz de la cookie VtexIdClientAutCookie para mantener sesión entre subdominios | Config de autenticación de la cuenta |
| **Duração do Refresh Token** (Identity) | /identity/set-refresh-token-session-duration | Define la duración de la sesión del refresh token (selector de scope, ej. Webstore) | Config de autenticación de la cuenta |
| **Habilitar Token Exchange (OAuth)** (Identity) | /identity/enable-oauth-token-exchange | Verifica y habilita la feature `oAuthAccessTokenExchange` en cuentas **headless** | Solo cuentas headless |
| **Validação de Cluster** (Storage, corrección) | /storage/masterdata | Corrige el estado del cluster de almacenamiento | Tras verificar que hay inconsistencia |
| **Profile System** (Storage) | /storage/profile-system-unification | Consulta y configura la unificación de clientes entre una cuenta y sus subaccounts (solo parent ↔ subaccount; las unificadas comparten la base de clientes del parent) | La unificación comparte datos de clientes |
| **Instalar Apps** (Assinaturas) | /subscriptions/install-apps | Valida e instala las apps del módulo de suscripciones en el **workspace master** y orienta sobre pasos de Intelligent Search | Instala en master |
| **User Translations** (Store Framework) | /store-framework/user-translations | Consulta y sobrescribe traducciones automáticas de `vtex.messages` en 3 contextos: Catálogo, App messages e Intelligent Search (facets). Flujo: datos del mensaje → resultado → editar → resultado | Edición visible en la tienda |
| **Ferramentas KI / transações** (Pagamentos) | /payments | Identifica y destraba transacciones atascadas por **Known Issues**. Una transacción o lote de hasta **50** (transactionId de 32 caracteres alfanuméricos); acepta listas (coma, punto y coma, espacio, salto de línea) o CSV; procesa en secuencia y se puede cancelar sin perder lo ya procesado. Un ícono (i) junto al título lista las KIs soportadas | Pide ID del ticket con cliente; destraba transacciones reales |
| **Reset do WebOps** (FastStore) | /webops/reset-onboarding | Elimina el proyecto WebOps de una cuenta para permitir un nuevo onboarding | **Solo para fases de implementación** |

## Mapa rápido: síntoma → herramienta

- Checkout / carrito raro → **OrderForm**, luego **Segurança** si hay bloqueos, **Simulação de fulfillment** si el problema es seller/flete.
- SKU no disponible o sin entrega en un CEP → **Delivery Promise**, luego **Catálogo** y **Simulação de fulfillment**.
- Producto que no aparece en la tienda o no se vende → **Catálogo**, luego **Intelligent Search**.
- Pedido atascado o con estado raro → **Resumo do pedido**.
- Transacción de pago atascada → revisar la lista de KIs soportadas en **Ferramentas KI / transações** (y el catálogo público, ver `vtex-fse-known-issues`); si coincide, destrabar (requiere ID del ticket).
- Rechazos de pago por tarjeta/emisor → **Consulta BIN**.
- Dominio, SSL, DNS → **CDN**.
- Página ausente del sitemap → **Sitemap**.
- Sesión que no se mantiene entre subdominios → **Domínio Raiz do Auth Cookie**.
- Site Editor / imágenes / FastStore fallando → **Validação de Cluster**.
- Fecha/hora o país de la cuenta incorrectos → **Timezone da conta**.
- Coordenadas de un CP incorrectas → **Geocoordenadas** (impacto global, ver advertencia).
- Un front-end roto en un workspace de desarrollo → **Apps instalados**, luego **Workspace** solo si se justifica aislar (irreversible).

## Límites y notas

- Límites de lote: Delivery Promise **20 líneas**; Pagamentos **50 transacciones**.
- Varias herramientas piden el **ID del ticket con cliente** por trazabilidad (Geocoordenadas, Pagamentos).
- Este resumen corresponde a la v3.0.0; la herramienta evoluciona. Si el agente menciona una opción que no está aquí, o algo no coincide, confía en lo que ve en la UI y sugiere actualizar este skill (ver `vtex-fse-knowledge-base`, sección de mantenimiento).

## Cómo redirigir al agente

Cuando una herramienta aplique, dilo de forma accionable y breve:

> Antes de escalar, corre **Delivery Promise** en el Troubleshooter (/delivery-promise) con SKU `123`, país, CEP y sales channel del pedido. El request/response que devuelve nos dice si el SKU falla en Delivery Promise, Intelligent Search o Checkout. Pégame el resultado y seguimos.

Si el caso requiere una herramienta que modifica configuración, indica explícitamente su alcance y pide confirmación al agente antes de sugerir ejecutarla.

## Relación con otros skills

- `vtex-fse-ticket-reading`: el Troubleshooter es una fuente de evidencia más del paso de análisis, junto con Slack y `vtex-fse-known-issues`.
- `vtex-fse-knowledge-base`: define qué es scope de FSE y a quién escalar; su sección de herramientas remite a este skill para el detalle del Troubleshooter.
- `vtex-fse-product-escalation`: los resultados de las herramientas se citan como evidencia y como "qué se descartó" en el escalation.
- `vtex-fse-client-writing`: reglas de redacción del texto final.
