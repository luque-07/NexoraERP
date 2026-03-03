# 🧠 Explicación completa de Nexora ERP

## 1️⃣ Concepto general

**Nexora ERP** es un **mini sistema de gestión empresarial** (ERP) desarrollado en **Java** con arquitectura por capas y enfoque profesional. Su objetivo es simular un ERP real, permitiendo gestionar **usuarios, productos, inventario, pedidos, pagos y reportes** sin necesidad de base de datos externa (se guarda todo en archivos).

El proyecto está diseñado siguiendo buenas prácticas profesionales:

* **POO avanzada:** herencia, interfaces, polimorfismo y encapsulamiento.
* **Principios SOLID:** cada clase y método tiene responsabilidad clara.
* **Persistencia:** permite guardar datos de usuarios, productos y pedidos para que se mantengan entre ejecuciones.
* **Control de acceso:** según el rol del usuario (Administrador o Cliente).
* **Colecciones y Streams:** para gestionar listas, mapas y cálculos de reportes.

---

## 2️⃣ Capas del proyecto y responsabilidades

### a) `model` — Clases de dominio

Aquí se definen **las entidades principales del sistema**.

**Clases principales:**

1. **`Usuario` (abstracta)**

   * Atributos: `id`, `nombre`, `email`, `contraseña`, `rol`.
   * Métodos: getters y setters, `toString()`.
   * Función: representar un usuario genérico del sistema.
2. **`Cliente`**

   * Hereda de `Usuario`.
   * Puede realizar pedidos y pagos.
3. **`Administrador`**

   * Hereda de `Usuario`.
   * Puede gestionar productos, stock y ver reportes.
4. **`Producto`**

   * Atributos: `id`, `nombre`, `descripcion`, `precio`, `stock`.
   * Función: representar artículos que pueden venderse.
5. **`Pedido`**

   * Atributos: `id`, `cliente`, `fecha`, `listaLineaPedido`, `estado` (ENUM).
   * Función: almacenar toda la información de un pedido de un cliente.
6. **`LineaPedido`**

   * Atributos: `producto`, `cantidad`, `precioUnitario`.
   * Función: detalle de cada producto dentro de un pedido.
7. **`EnumEstadoPedido`**

   * Valores: `PENDIENTE`, `PAGADO`, `ENVIADO`, `CANCELADO`.
   * Función: controlar el flujo de un pedido.
8. **`Pago` (opcional como clase base)**

   * Para integrar con las interfaces de pago si se desea almacenar historial.

---

### b) `service` — Lógica de negocio

Aquí se implementan **las acciones y reglas de negocio**, sin tocar cómo se guardan los datos.

1. **`UsuarioService`**

   * Métodos:

     * `registrarUsuario()`: crea Cliente o Administrador.
     * `login(email, contraseña)`: verifica credenciales.
     * `listarUsuarios()`: lista todos los usuarios registrados.
   * Función: centralizar toda la lógica de usuarios y roles.
2. **`ProductoService`**

   * Métodos:

     * `crearProducto()`
     * `modificarProducto()`
     * `eliminarProducto()`
     * `buscarProducto()`
     * `controlarStock()`
   * Función: gestionar el catálogo de productos y stock.
3. **`PedidoService`**

   * Métodos:

     * `crearPedido(cliente)`
     * `agregarProducto(pedido, producto, cantidad)`
     * `calcularTotal(pedido)`
     * `cambiarEstado(pedido, estado)`
   * Función: gestionar pedidos y validar stock antes de confirmar.
4. **`PagoService`**

   * Método:

     * `realizarPago(Pedido pedido, MetodoPago metodoPago)`
   * Función: procesar pagos usando **polimorfismo** para aceptar tarjeta, PayPal o transferencia.

---

### c) `repository` — Persistencia en memoria y archivos

Encapsula **cómo se guardan y recuperan los datos**.

* `UsuarioRepository` → maneja lista o mapa de usuarios.
* `ProductoRepository` → maneja productos y stock.
* `PedidoRepository` → maneja pedidos activos e históricos.

💡 Los repositorios **no contienen lógica de negocio**, solo **CRUD y carga/guardado**.

---

### d) `exception` — Manejo de errores personalizados

* `UsuarioNoEncontradoException` → login o búsqueda fallida.
* `StockInsuficienteException` → al pedir más productos que los disponibles.
* `PedidoNoEncontradoException` → si un pedido no existe.
* `PagoFallidoException` → errores en procesamiento de pagos.

💡 Esto permite **controlar errores de manera profesional**, sin romper la ejecución del programa.

---

### e) `util` — Herramientas y utilidades

* `FileManager` → guardar y cargar usuarios, productos y pedidos en archivos `.ser` o CSV.
* `InputValidator` → validar emails, contraseñas, números y fechas.
* `IdGenerator` → generar IDs únicos automáticos para productos, pedidos y usuarios.

---

### f) `payment` — Polimorfismo de pagos

* `MetodoPago` → interfaz base con `procesarPago(double cantidad)`.
* Implementaciones:

  * `PagoTarjeta`
  * `PagoPaypal`
  * `PagoTransferencia`
* Permite **añadir nuevos métodos de pago** sin tocar el código existente, siguiendo el principio **Open/Closed** de SOLID.

---

## 3️⃣ Flujo completo del programa

1. El usuario inicia `App.java` (main) → muestra **menú principal**.
2. **Login**:

   * Se verifica si es **Administrador** o **Cliente**.
   * Según rol, muestra las opciones correspondientes.
3. **Administrador**:

   * Gestiona usuarios (si quieres).
   * Gestiona productos: crear, modificar, eliminar.
   * Visualiza reportes: ventas totales, producto más vendido, cliente más activo.
4. **Cliente**:

   * Visualiza productos disponibles.
   * Crea pedidos:

     * Selecciona productos y cantidades.
     * Se valida stock automáticamente.
   * Realiza pagos usando cualquier método implementado.
5. **Persistencia**:

   * Cada acción crítica guarda los datos en archivos (`FileManager`).
   * Al iniciar el programa, los datos se cargan automáticamente.
6. **Reportes**:

   * Se calculan usando colecciones y Streams.
   * Se pueden obtener estadísticas de ventas, ingresos y clientes.

---

## 4️⃣ Buenas prácticas aplicadas

* **No hay código en `main`** salvo menú y orquestación.
* **Encapsulamiento total:** atributos privados, métodos públicos necesarios.
* **Métodos pequeños y claros**, cada uno hace una sola cosa.
* **Polimorfismo real:** en pagos y usuarios.
* **Excepciones propias** para errores específicos.
* **Persistencia segura** con archivos, evitando base de datos externa (fase inicial).
* **Fácil ampliación:** nuevas fases, métodos de pago, reportes o conexión a base de datos.

---