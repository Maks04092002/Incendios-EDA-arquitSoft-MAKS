# C4 Nivel 3: Diagrama de Componentes (Components)

## 1. Descripción General
Descompone el contenedor **API Backend (Node.js / Express)** en sus módulos de negocio internos y sus conexiones.

---

## 2. Diagrama de Componentes del API Backend

```mermaid
graph TB
    WEB["💻 Aplicación Web"]

    subgraph Backend ["📦 API Backend (Node.js / Express)"]
        AUTH["🔐 Controller / Router JWT"]

        subgraph Modulos ["🧩 Módulos Internos"]
            USR["👤 Módulo Usuarios"]
            SLR["🏬 Módulo Sellers"]
            CAT["📦 Módulo Catálogo"]
            CAR["🛒 Módulo Carrito"]
            PED["📋 Módulo Pedidos"]
            PAG["💳 Módulo Pagos"]
            ENV["🚚 Módulo Envíos"]
        end
    end

    DB[("🗄️ PostgreSQL")]
    CACHE[("⚡ Redis")]
    EXT["☁️ Servicios Externos (Pagos/ERP/Envíos)"]

    WEB -->|REST / JSON| AUTH
    AUTH --> USR & SLR & CAT & CAR & PED & PAG & ENV

    CAT <--> CACHE
    CAR <--> CACHE

    PED --> PAG
    PED --> ENV

    USR & SLR & CAT & CAR & PED & PAG & ENV --> DB
    PAG & PED & ENV --> EXT

    style Backend fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px
```

---

## 3. Detalle de Componentes Internos

| Componente | Función Principal |
| :--- | :--- |
| **Router / Auth** | Autenticación mediante Tokens JWT y enrutamiento de peticiones. |
| **Módulo Usuarios** | Registro, autenticación y roles de acceso. |
| **Módulo Catálogo** | Gestión de productos para mascotas y sincronización de stock con Redis. |
| **Módulo Carrito** | Manejo de ítems seleccionados temporalmente por el cliente. |
| **Módulo Pedidos** | Orquestación central de órdenes de compra. |
| **Módulo Pagos** | Integración transaccional con la Pasarela de Pagos externa. |
| **Módulo Envíos** | Asignación de rutas y despacho con operadores logísticos. |