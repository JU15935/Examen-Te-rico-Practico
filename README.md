# Examen-Te-rico-Practico

---
config:
  theme: neutral
---
classDiagram
    class Persona {
        -String nombre
        -String telefono
        +getContacto() String
    }
    
    class Empleado {
        -String puesto
        +registrarEntrada() Date
        +asignarMesa(Mesa) boolean
    }
    
    class Cliente {
        -String direccion
        +hacerReserva() boolean
        +realizarPedido() Pedido
    }
    
    class Mesa {
        -int numero
        -int capacidad
        -boolean disponible
        +reservar() boolean
        +liberar() void
        +getEstado() String
    }
    
    class Pedido {
        -Date fechaHora
        -String estado
        -float total
        +calcularTotal() float
        +agregarProducto(Producto) void
        +eliminarProducto(Producto) void
        +confirmar() boolean
    }
    
    class Producto {
        -String nombre
        -float precio
        -int stock
        +getInfo() String
        +aplicarDescuento(float) void
    }
    
    class Categoria {
        -String nombre
        -String descripcion
        +getProductos() List~Producto~
    }

    %% Relaciones de herencia
    Persona <|-- Empleado : es un
    Persona <|-- Cliente : es un
    
    %% Asociaciones
    Empleado "1" -- "0..*" Mesa : asigna >
    Cliente "1" -- "0..*" Pedido : realiza >
    Producto "1" -- "1" Categoria : pertenece a >
    
    %% Composición
    Mesa "1" *-- "0..*" Pedido : contiene >
    
    %% Agregación
    Pedido "1" o-- "1..*" Producto : incluye >

    Análisis del Sistema de Gestión de Restaurantes
He representado el sistema de gestión de restaurantes según el enunciado, utilizando un diagrama de clases UML. A continuación, analizaré las entidades principales y las relaciones entre ellas, explicando cómo colaboran para crear un flujo coherente de operaciones en el restaurante.
Entidades y sus Responsabilidades
1. Persona (Clase Base)

Representa los datos comunes de cualquier individuo que interactúa con el sistema.
Contiene atributos básicos como nombre y teléfono.
Implementa getContacto() para obtener la información de contacto.

2. Empleado (Hereda de Persona)

Especialización de Persona que representa al personal del restaurante.
Añade el atributo puesto para indicar su función en el restaurante.
Puede registrar su entrada al trabajo mediante registrarEntrada().
Asigna y gestiona mesas a través de una relación de asociación.

3. Cliente (Hereda de Persona)

Otra especialización de Persona que representa a los comensales.
Añade el atributo direccion para entregas a domicilio o facturación.
Puede hacer reservas con hacerReserva() y realizar pedidos.
Se asocia con múltiples pedidos, creando un historial de consumo.

4. Mesa

Representa un espacio físico donde los clientes pueden ser atendidos.
Incluye atributos como numero, capacidad y disponible.
Mantiene una relación de composición con los pedidos, lo que significa que si una mesa se elimina, todos sus pedidos asociados también se eliminan.

5. Pedido

Representa una orden completa realizada por un cliente.
Registra fechaHora y estado (ej: pendiente, en preparación, servido, pagado).
Puede calcular el total mediante calcularTotal().
Mantiene una relación de agregación con productos, permitiendo añadir o quitar items.

6. Producto

Representa los diferentes ítems del menú que se pueden ordenar.
Incluye información como nombre, precio y stock.
Proporciona métodos como getInfo() para obtener detalles y aplicarDescuento().
Se puede reutilizar en múltiples pedidos sin afectar su existencia.

7. Categoría

Clasifica los productos en grupos lógicos (entrantes, platos principales, postres, etc.).
Cada producto pertenece exactamente a una categoría.

Relaciones Clave

Herencia:

Persona es la clase base para Empleado y Cliente, implementando un diseño "es un".


Asociación:

Empleado asigna múltiples Mesas (relación 1 a muchos).
Cliente realiza múltiples Pedidos (relación 1 a muchos).
Producto pertenece a una Categoría (relación muchos a 1).


Composición:

Mesa contiene Pedidos (relación 1 a muchos). Si se elimina la mesa, los pedidos asociados también se eliminan.


Agregación:

Pedido incluye uno o más Productos (relación 1 a muchos). Los productos existen independientemente de los pedidos.



Flujo de Operaciones en el Restaurante

Llegada del Cliente:

Un Cliente (subclase de Persona) llega al restaurante o llama para hacer una reserva.
Utiliza el método hacerReserva() para solicitar una mesa.


Asignación de Mesa:

Un Empleado verifica la disponibilidad de Mesas.
El empleado asigna una mesa al cliente (relación de asociación).
La mesa cambia su estado a reservada mediante el método reservar().


Creación de Pedido:

Una vez sentado, el Cliente puede realizar un Pedido (relación de asociación).
El pedido se asocia automáticamente con la Mesa asignada (relación de composición).


Selección de Productos:

El pedido incorpora uno o más Productos del menú (relación de agregación).
Cada producto pertenece a una Categoría específica, facilitando la organización del menú.
El pedido va sumando los precios de los productos mediante calcularTotal().


Facturación y Pago:

Cuando el cliente termina, el sistema calcula el total del pedido.
El pedido cambia su estado a "pagado".
La mesa puede liberarse para nuevos clientes con el método liberar().
