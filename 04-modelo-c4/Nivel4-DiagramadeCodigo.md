# C4 Nivel 4: Diagrama de Código (Code)

## 1. Descripción General
Muestra el diseño detallado de código dentro del módulo **Pedidos** aplicando principios de **Clean Architecture** y patrones de diseño.

---

## 2. Diagrama de Clases (Módulo Pedidos)

```mermaid
classDiagram
    class OrderController {
        -crearPedidoUseCase: CrearPedidoUseCase
        +crear(req, res): Promise~void~
    }

    class CrearPedidoUseCase {
        -orderRepository: OrderRepository
        -paymentPort: PaymentPort
        +ejecutar(cmd: CrearPedidoCommand): Promise~string~
    }

    class Pedido {
        <<Entity>>
        -id: string
        -clienteId: string
        -items: ItemPedido[]
        -estado: EstadoPedido
        +crear(clienteId, items)$ Pedido
        +total(): number
        +marcarPagado(autorizacion): void
    }

    class OrderRepository {
        <<Interface>>
        +guardar(pedido: Pedido): Promise~void~
    }

    class PaymentPort {
        <<Interface>>
        +cobrar(monto: number, token: string): Promise~ResultadoCobro~
    }

    class PostgresOrderRepository {
        -db: Pool
        +guardar(pedido: Pedido): Promise~void~
    }

    class StripePaymentAdapter {
        -stripe: Stripe
        +cobrar(monto: number, token: string): Promise~ResultadoCobro~
    }

    OrderController ..> CrearPedidoUseCase
    CrearPedidoUseCase --> OrderRepository
    CrearPedidoUseCase --> PaymentPort
    CrearPedidoUseCase ..> Pedido : «crea»
    PostgresOrderRepository ..|> OrderRepository
    StripePaymentAdapter ..|> PaymentPort
```

---

## 3. Diagrama de Secuencia (Flujo de Creación de Pedido)

```mermaid
sequenceDiagram
    autonumber
    actor Cliente as Cliente web
    participant Ctrl as :OrderController
    participant UC as :CrearPedidoUseCase
    participant Ped as :Pedido
    participant Pay as :PaymentPort
    participant Repo as :OrderRepository

    Cliente->>Ctrl: POST /api/v1/pedidos
    Ctrl->>UC: ejecutar(cmd)
    UC->>Ped: crear(clienteId, items)
    Ped-->>UC: pedido [PENDIENTE]
    UC->>Pay: cobrar(monto, token)
    Pay-->>UC: ResultadoCobro (aprobado)
    UC->>Ped: marcarPagado(autorizacion)
    UC->>Repo: guardar(pedido)
    Repo-->>UC: ok
    UC-->>Ctrl: pedidoId
    Ctrl-->>Cliente: 201 Created {pedidoId}
```