# Api's

La API de Chink es REST y responde en JSON. Cada endpoint tiene una función concreta y se llama con un método HTTP (GET, POST, PATCH o DELETE). La dirección base en producción es `https://chink.relake.co/api`.

Casi todos los endpoints necesitan que el usuario haya iniciado sesión. En ese caso hay que mandar el token en el encabezado:

```http
Authorization: Bearer <token>
```

Si no se manda el token, o si ya venció, la API responde `401`. Cuando el usuario no tiene el permiso que pide la ruta, responde `403`. Los errores siempre vienen en este formato:

```json
{ "error": "Mensaje que explica qué pasó" }
```

## Autenticación (`/api/auth`)

* `POST /api/auth/login`: inicia sesión con correo y contraseña. Devuelve el token y los datos del usuario.
* `POST /api/auth/register`: crea una cuenta nueva con su propio espacio de trabajo.
* `PATCH /api/auth/activate`: activa la cuenta de alguien invitado, cambiando la clave temporal por una contraseña propia.
* `PATCH /api/auth/password`: cambia la contraseña del usuario que tiene la sesión abierta.
* `GET /api/auth/me`: devuelve los datos del usuario de la sesión, con su rol y sus permisos.

## Inventario (`/api/inventory`)

* `GET /api/inventory/summary`: stock total, disponible, reservado, valor total del inventario, conteos y almacenes.
* `GET /api/inventory/products`: lista de productos. Acepta los filtros `search`, `category`, `warehouse`, `status` y `view` (por ejemplo `view=archived`).
* `GET /api/inventory/products/:id`: detalle de un producto.
* `POST /api/inventory/products`: crea un producto.
* `PATCH /api/inventory/products/:id`: modifica un producto o lo archiva.
* `PATCH /api/inventory/products/images`: cambia la imagen de uno o varios productos.
* `POST /api/inventory/product-images/upload-urls`: pide las direcciones para subir imágenes.
* `GET /api/inventory/product-images/status`: dice si el almacenamiento de imágenes está activo.

## Movimientos (`/api/movements`)

* `POST /api/movements`: registra una entrada, salida, transferencia o ajuste.
* `GET /api/movements`: historial de movimientos, con filtros por tipo, almacén, fecha y búsqueda.
* `GET /api/movements/summary`: totales de entradas y salidas.
* `GET /api/movements/weekly`: movimientos por día de la semana, para la gráfica de Inicio.

## Almacenes (`/api/warehouses`)

* `GET /api/warehouses`: lista de almacenes.
* `POST /api/warehouses`: crea un almacén.
* `GET /api/warehouses/locations`: existencias de cada producto por almacén.
* `GET /api/warehouses/summary`: resumen de ocupación de cada almacén.

## Equipo (`/api/team`)

* `GET /api/team/members`: personas del espacio con su rol y estado.
* `POST /api/team/invitations`: invita a una persona y genera su clave temporal. Solo la Administradora.
* `GET /api/team/permissions`: matriz de roles y permisos.
* `PATCH /api/team/permissions`: cambia qué puede hacer cada rol. Solo la Administradora.
* `GET /api/team/spaces`: espacios a los que pertenece el usuario.
* `PATCH /api/team/active-space`: cambia el espacio activo.

## Reportes (`/api/reports`)

* `GET /api/reports/summary`: stock al cierre, entradas, salidas y variación del período.
* `GET /api/reports/categories`: stock agrupado por categoría.
* `GET /api/reports/low-stock`: productos en stock bajo.
* `GET /api/reports/exports`: lista de reportes que se pueden descargar.
* `POST /api/reports/email`: envía un reporte por correo.

## Configuración y notificaciones

* `GET /api/settings`: datos y reglas del espacio de trabajo.
* `PATCH /api/settings`: modifica la configuración del espacio.
* `GET /api/notifications`: notificaciones del usuario.
* `PATCH /api/notifications/read`: marca las notificaciones como leídas.
* `GET /api/stream`: canal de eventos para que la pantalla se actualice sola cuando otro usuario hace un cambio.

## Pagos y clientes (`/api/pagos`, `/api/clientes`)

* `GET /api/pagos`: lista de comprobantes de pago.
* `GET /api/pagos/semana`: totales de la semana.
* `GET /api/pagos/:id`: detalle de un pago.
* `POST /api/pagos`: registra un pago a mano.
* `PATCH /api/pagos/:id`: modifica un pago o cambia su estado.
* `DELETE /api/pagos/:id`: borra un pago.
* `GET /api/pagos/cierre/resumen`: cierre de caja del día.
* `GET /api/clientes`, `POST /api/clientes`, `PATCH /api/clientes/:id` y `DELETE /api/clientes/:id`: manejo de clientes.

## Telegram (`/api/telegram`)

* `GET /api/telegram/estado`: dice si el bot está activo y qué chats están vinculados.
* `POST /api/telegram/codigo`: genera un código para vincular un chat con `/vincular CODIGO`.
* `DELETE /api/telegram/chats/:chatId`: desvincula un chat.
* `POST /api/telegram/webhook`: lo llama Telegram cada vez que alguien le escribe al bot.
