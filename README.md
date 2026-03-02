# Nexora ERP

**Mini ERP profesional en Java con arquitectura por capas, POO avanzada y persistencia en archivos.**

---

## Objetivo

Crear un sistema que gestione:

- Usuarios
- Productos
- Inventario
- Pedidos
- Pagos
- Reportes
- Persistencia en archivos
- Control de acceso por roles

---

## Fases del proyecto

### Fase 1 — Sistema de Usuarios
- Clase abstracta `Usuario` y subclases `Cliente` y `Administrador`
- Login, roles y validación de contraseña
- Guardado de usuarios en archivo

### Fase 2 — Gestión de Productos
- Crear, modificar, eliminar y buscar productos
- Control de stock
- Uso de colecciones (`ArrayList`, `HashMap`)

### Fase 3 — Sistema de Pedidos
- `Pedido` con ID, cliente, fecha, lista de `LineaPedido` y estado
- Agregar productos, calcular total y validar stock

### Fase 4 — Sistema de Pagos
- Interfaz `MetodoPago`
- Implementaciones: `PagoTarjeta`, `PagoPaypal`, `PagoTransferencia`
- Polimorfismo

### Fase 5 — Persistencia
- Guardar productos, pedidos y usuarios en archivos
- Cargar datos al iniciar el programa
- Serialización o CSV

### Fase 6 — Reportes
- Total ventas por día
- Producto más vendido
- Cliente que más compra
- Ingresos totales
- Uso de Streams y colecciones

---

## Tecnologías y conceptos
- Java 17+ (recomendado)
- Eclipse IDE
- POO avanzada, Herencia, Polimorfismo
- Principios SOLID
- Excepciones personalizadas
- Serialización y manejo de archivos
- Patrones de diseño básicos (Factory, Singleton)

---

## Estructura del proyecto

```text
com.empresa
├── main
├── model
├── service
├── repository
├── exception
├── util
└── payment
```
--- 
## Como usuar

Clonar el repositorio:

git clone https://github.com/tuUsuario/NexoraERP.git

Abrir en Eclipse como proyecto Java.

Ejecutar main.App.java.

Seguir el menú para usuarios, productos, pedidos y pagos.