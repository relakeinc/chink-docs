# Controladores Backend

En Fastify cada módulo registra sus rutas en un archivo dentro de `apps/api/src/routes/`. Aquí explicamos los controladores más importantes del sistema: qué recibe cada endpoint, qué hace por dentro y qué responde.

Todos (menos el de login) necesitan el token en el encabezado `Authorization: Bearer <token>`.

## AuthController (`routes/auth.ts`)

### Iniciar sesión

`POST /api/auth/login`

Recibe el correo y la contraseña del usuario.

```json
{
  "email": "lucia@chink.es",
  "password": "chink2026"
}
```

Operaciones internas:

1. Busca el usuario por su correo.
2. Compara la contraseña con el hash guardado usando bcrypt.
3. Si el correo no existe o la contraseña no coincide, responde `401` con el mismo mensaje en los dos casos, para no dar pistas de qué cuentas existen.
4. Arma la sesión con el espacio activo, el rol y los permisos del usuario.
5. Guarda la fecha del último acceso y firma el token JWT.

Respuesta:

```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": "cm3x7u5s0000508l4",
    "email": "lucia@chink.es",
    "name": "Lucía Martín",
    "initials": "LM",
    "role": "Administradora",
    "status": "ACTIVE",
    "workspaceId": "default"
  },
  "permissions": ["consult", "register", "adjust", "report", "admin"]
}
```

Con datos incorrectos:

```json
{ "error": "Correo o contraseña incorrectos" }
```

## InventoryController (`routes/inventory.ts`)

### Crear un producto

`POST /api/inventory/products`

```json
{
  "sku": "GRA-001",
  "name": "Arroz Diana 500 g",
  "category": "Granos",
  "stock": 20,
  "unitCost": 2800,
  "minStock": 5,
  "warehouseId": "cm3x8k2p0000108l4"
}
```

Operaciones internas:

1. Valida el cuerpo con el esquema: el nombre es obligatorio y el stock y el costo no pueden ser negativos.
2. Revisa que no exista otro producto con el mismo SKU en el espacio. Si existe, responde `409` con el mensaje "El SKU GRA-001 ya existe".
3. Calcula el valor total (`stock × unitCost`) y el estado (`IN_STOCK` o `LOW_STOCK` según el mínimo).
4. Crea el producto y su existencia en el almacén indicado (`stock_locations`).
5. Avisa a las demás pantallas abiertas del espacio para que se actualicen.

Respuesta (`201`):

```json
{
  "data": {
    "id": "cm3x9a1b2000208l4",
    "sku": "GRA-001",
    "name": "Arroz Diana 500 g",
    "category": "Granos",
    "stock": 20,
    "unitCost": 2800,
    "totalValue": 56000,
    "status": "IN_STOCK"
  }
}
```

### Resumen del inventario

`GET /api/inventory/summary`

Suma el stock, lo reservado y el valor de todos los productos del espacio, y cuenta cuántos están en stock bajo y cuántos archivados.

```json
{
  "totalStock": 25,
  "available": 25,
  "reserved": 0,
  "totalValue": 13000,
  "counts": { "all": 2, "lowStock": 0, "archived": 0 },
  "categories": ["Granos", "Lácteos"],
  "warehouses": [{ "id": "cm3x8k2p0000108l4", "code": "ALM-001", "name": "Bodega principal" }]
}
```

## MovementController (`routes/movements.ts`)

### Registrar un movimiento

`POST /api/movements`

El campo `type` puede ser `ENTRY` (entrada), `EXIT` (salida), `TRANSFER` (transferencia) o `ADJUSTMENT` (ajuste). Según el tipo se manda `originId`, `destinationId` o los dos.

```json
{
  "type": "EXIT",
  "productId": "cm3x9a1b2000208l4",
  "quantity": 2,
  "originId": "cm3x8k2p0000108l4",
  "note": "Venta de mostrador"
}
```

Operaciones internas:

1. Valida que la cantidad sea un número entero mayor que cero.
2. Revisa que el producto y los almacenes existan y sean del mismo espacio.
3. Si es una salida o una transferencia, revisa que en el almacén de origen haya suficientes unidades. Si no hay, responde `409` y no cambia nada.
4. Dentro de una transacción, actualiza la existencia en cada almacén, el stock total del producto, su valor y su estado, y guarda el movimiento con el usuario que lo hizo.
5. Si el producto quedó en su stock mínimo o por debajo, lo marca como stock bajo y lo devuelve en `crossedLowStock` para que la pantalla muestre la alerta.

Respuesta (`201`):

```json
{
  "data": {
    "id": "cm3xa0c7d000308l4",
    "ref": "MOV-0013",
    "type": "Salida",
    "product": "Arroz Diana 500 g",
    "sku": "GRA-001",
    "quantity": 2,
    "owner": "Ana López",
    "status": "Confirmado"
  },
  "stockDelta": -2,
  "crossedLowStock": null
}
```

Si no hay suficientes unidades:

```json
{ "error": "No hay stock suficiente en Bodega principal (3 u.)" }
```

### Historial de movimientos

`GET /api/movements?type=Salida&search=GRA-001`

Devuelve los movimientos del más reciente al más antiguo, con la fecha, la hora, el tipo, la cantidad, los almacenes y el usuario. Los movimientos no se pueden editar ni borrar, por eso este controlador no tiene rutas PATCH ni DELETE.

## TeamController (`routes/team.ts`)

### Invitar a un miembro

`POST /api/team/invitations` (solo con el permiso `admin`)

```json
{
  "name": "Pablo Vidal",
  "email": "pablo@tienda.com",
  "roleId": "cm3x7r0l0000008l4",
  "warehouseIds": ["cm3x8k2p0000108l4"]
}
```

Operaciones internas:

1. Revisa que quien invita tenga el permiso `admin`.
2. Revisa que el rol exista. Si no, responde `400` con "Rol no válido".
3. Si la persona ya tiene cuenta en Chink, la agrega al espacio con ese rol y le manda una notificación.
4. Si es nueva, crea el usuario en estado pendiente con el rol y los almacenes elegidos, y genera una clave temporal que la persona usa en la pantalla de activación.
5. Si el correo está configurado, le envía el aviso por correo.

## WarehouseController (`routes/warehouses.ts`)

### Crear un almacén

`POST /api/warehouses`

```json
{
  "code": "ALM-002",
  "name": "Bodega de atrás",
  "location": "Cali",
  "address": "Cra. 5 # 12-30",
  "capacity": 500
}
```

Revisa que el código no esté repetido dentro del espacio y guarda el almacén. Desde ese momento ya se pueden registrar movimientos hacia él o desde él.

## Observaciones del equipo de pruebas

En el primer ciclo de pruebas encontramos que algunas rutas que modifican datos (`POST /api/inventory/products`, `PATCH /api/inventory/products/:id`, `POST /api/movements` y `POST /api/warehouses`) todavía no usan `requirePermission`. El detalle está en el reporte de defectos del proyecto y se corrige en el siguiente ciclo.
