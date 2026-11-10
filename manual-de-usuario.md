# Manual de usuario

Chink es un sistema web para manejar el inventario de una tienda pequeña: qué productos hay, cuántas unidades quedan en cada almacén, qué entra y qué sale, y cuánto vale todo. No hay que instalar nada, se usa desde el navegador del computador o del celular en **chink.relake.co**.

Este manual está pensado para el dueño de la tienda y para sus empleados. Va pantalla por pantalla, en el mismo orden del menú.

## 1. Cómo entrar

**Si es la primera vez (dueño de la tienda):**

1. Entre a chink.relake.co y haga clic en **"Crea una aquí"**.
2. Escriba su nombre, su correo y una contraseña de mínimo 8 caracteres. Recomendamos usar letras, números y algún símbolo.
3. El sistema le pide crear su **espacio**, que es como el "local" dentro de Chink. Ponga el nombre del negocio, un correo de contacto y, si quiere, la razón social y el NIT. Al terminar le da clic a **Crear espacio**.

**Si lo invitaron (empleado):**

El dueño le da una **clave temporal**. Entre a la pantalla de activación, escriba esa clave y luego su nueva contraseña dos veces. Desde ahí ya entra normal con su correo y su contraseña.

**Las siguientes veces:** entre a chink.relake.co, escriba su correo y contraseña y déle a **Entrar**. Para salir, use la opción de cerrar sesión en Configuración, sobre todo si está en un computador que usan otras personas.

## 2. Roles: qué puede hacer cada persona

Cada persona del equipo tiene un rol, y según el rol el menú le muestra unas opciones u otras. Hay cuatro:

* **Administradora:** puede hacer todo. Es el rol del dueño de la tienda, porque es el único que puede invitar personas, cambiar roles y entrar a Configuración.
* **Gestor:** puede consultar, registrar entradas y salidas, hacer ajustes de stock, manejar los almacenes y ver los reportes. Lo único que no puede es manejar el equipo ni la configuración. Sirve para un encargado o administrador del local.
* **Operador:** consulta el inventario y registra entradas, salidas y transferencias. No puede hacer ajustes ni ver reportes. Es el rol típico del empleado de caja o de bodega.
* **Consulta:** solo puede mirar el inventario, los movimientos y los reportes. No puede cambiar nada. Sirve, por ejemplo, para un contador o un socio que solo quiere revisar.

Además, a cada persona se le pueden asignar solo algunos almacenes, para que vea únicamente los suyos.

## 3. Pantalla de Inicio y menú

Apenas entra, el sistema lo lleva a **Inicio**, que es un resumen de cómo va el inventario. Ahí se ve el stock total, cuántas unidades están disponibles y cuántas reservadas, cuántos productos están en stock bajo y una gráfica con las entradas y salidas de la semana. Desde Inicio también está el botón **Registrar movimiento**, para no tener que ir a otra pantalla cuando llega o sale mercancía.

**En el computador** el menú está a la izquierda: Inicio, Inventario, Movimientos, Almacenes, Pagos, Reportes, Equipo y Configuración. **En el celular** el menú aparece abajo con Inicio, Inventario, Pagos y **Más**, donde están las demás opciones.

Arriba hay un buscador para encontrar productos rápido, una campana con las notificaciones (por ejemplo, cuando un producto quedó en stock bajo) y, a la izquierda, el nombre del espacio.

## 4. Inventario

Aquí está la lista de todos los productos con su SKU (el código interno), categoría, stock y estado. Arriba de la lista sale el stock total, lo disponible, lo reservado y el **valor total** del inventario en pesos. La lista tiene tres pestañas: Todos los productos, Stock bajo y Archivados.

**Agregar un producto**

1. Haga clic en **Añadir producto**.
2. Llene el SKU (por ejemplo GRA-001), el nombre, la categoría, el costo por unidad en pesos, el stock inicial y el almacén donde va a quedar. Si quiere, puede subir una foto.
3. Guarde. El SKU no se puede repetir, si ya existe otro producto con ese código el sistema le avisa.

El precio y el stock no pueden ser negativos. Si los escribe mal, el sistema no deja guardar.

**Buscar y filtrar:** escriba parte del nombre o del SKU en el buscador, no importa si es en mayúsculas o minúsculas. También puede filtrar por categoría, por almacén o por estado.

**Acciones con productos seleccionados:** marque uno o varios productos de la lista y arriba aparecen las opciones **Cambiar imagen**, **Ajustar stock** e **Imprimir etiqueta**.

**Importar muchos productos a la vez:** con el botón **Importar** se sube un archivo CSV, que se puede hacer en Excel guardándolo como "CSV separado por punto y coma". Cada fila debe tener, en este orden: SKU; nombre; categoría; stock; reservado; costo por unidad. Por ejemplo:

```
GRA-001;Arroz Diana 500 g;Granos;20;0;2800
LAC-001;Leche Colanta 1 L;Lácteos;12;0;4200
```

**Exportar:** el botón **Exportar** descarga el inventario en un archivo CSV que se abre en Excel.

## 5. Movimientos

Cada vez que algo entra, sale o se mueve se registra un **movimiento**, y así el stock se actualiza solo. Hay cuatro tipos:

* **Entrada:** cuando llega mercancía del proveedor. Suma unidades.
* **Salida:** cuando se vende o se saca un producto. Resta unidades.
* **Transferencia:** cuando se pasa mercancía de un almacén a otro. El total no cambia, solo cambia dónde está.
* **Ajuste:** para corregir el stock cuando el conteo físico no coincide con el sistema, por ejemplo por un producto dañado o perdido. Solo lo pueden hacer la Administradora y el Gestor.

**Registrar un movimiento**

1. Haga clic en **Registrar movimiento**, ya sea en Inicio o en Movimientos.
2. Escoja el tipo, el producto y la cantidad.
3. Escoja el almacén. En una entrada es el almacén de destino, en una salida el de origen, y en una transferencia los dos.
4. Si quiere, escriba una nota, por ejemplo "Factura 1520 del proveedor".
5. Guarde.

El sistema no deja sacar más unidades de las que hay en ese almacén. Si lo intenta, sale un mensaje de que no hay stock suficiente y el stock no cambia.

**Historial:** la pantalla de Movimientos muestra todo lo que se ha registrado, del más reciente al más antiguo, con la fecha, la hora, el tipo, la cantidad y el usuario que lo hizo. Los movimientos no se pueden editar ni borrar. Si alguien se equivocó, se corrige con un movimiento nuevo.

## 6. Almacenes, alertas, pagos y reportes

**Almacenes.** Un almacén es cualquier lugar donde se guarda mercancía, como el local o la bodega. Para crear uno se usa **Añadir almacén**, poniendo un código (por ejemplo ALM-002), el nombre, la dirección, la ciudad y la capacidad en unidades.

**Alertas de stock bajo.** Cuando un producto llega a su stock mínimo, el sistema lo marca como **stock bajo**. Aparece en la pestaña Stock bajo del Inventario y en Inicio, y llega una notificación a la campana. Cuando se repone, la alerta se quita sola.

**Pagos.** Sirve para llevar el control de los comprobantes de pago que mandan los clientes por transferencia. Se pueden registrar con el bot de Telegram (se vincula el chat escribiendo `/vincular CODIGO` y luego se le mandan las fotos de los comprobantes) o a mano. Al final del día, en **Cierre del día**, se escribe el saldo real de la cuenta y el sistema muestra la diferencia.

**Reportes.** Muestra el stock al cierre, las entradas y salidas del período y el stock por categoría. Con **Exportar resumen** se descarga en CSV o PDF, y con **Enviar por correo** le llega a quien usted diga.

## 7. Equipo y configuración

Estas dos pantallas solo las ve la Administradora.

**Equipo.** Para agregar a alguien se usa **Invitar miembro**: se escribe el nombre y el correo, se escoge el rol y los almacenes, y el sistema genera una clave temporal para que la persona active su cuenta.

**Configuración.** Aquí se cambian los datos del espacio, el tema de la aplicación, los avisos y las reglas del inventario. También está la opción de **cambiar la contraseña** y de cerrar la sesión.

## 8. Preguntas frecuentes

**Olvidé mi contraseña, ¿qué hago?** Pídale a la Administradora que lo vuelva a invitar para que le genere una clave temporal nueva.

**No me aparece una opción del menú.** Eso depende de su rol. Si cree que la necesita, háblelo con la Administradora.

**Quise registrar una venta y me salió que no hay stock suficiente.** En ese almacén no hay tantas unidades. Revise si el producto está en otro almacén y haga primero una transferencia.

**El stock del sistema no coincide con lo que hay en la bodega.** Cuente el producto y haga un **ajuste** con la diferencia, explicando el motivo en la nota.

**¿Funciona en el celular?** Sí, se abre desde el navegador del celular en chink.relake.co, sin instalar nada.
