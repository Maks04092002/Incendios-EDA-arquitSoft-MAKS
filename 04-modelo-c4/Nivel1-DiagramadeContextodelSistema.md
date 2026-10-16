# C4 Nivel 1: Diagrama de Contexto del Sistema (System Context)

## 1. Descripción General
El Diagrama de Contexto proporciona una visión de alto nivel del **Marketplace de Productos para Mascotas (GoPet)**, mostrando las interacciones entre los diferentes usuarios principales y los sistemas externos con los que se integra la plataforma.

---

## 2. Diagrama de Contexto

```mermaid
graph TB
    subgraph Usuarios ["👥 Usuarios del Sistema"]
        C["📱 Cliente<br/>(Busca y compra productos)"]
        S["🏪 Vendedor / Seller<br/>(Publica productos y gestiona pedidos)"]
        A["🛡️ Administrador<br/>(Gestiona la plataforma y usuarios)"]
    end

    SYS["📦 Marketplace GoPet<br/>[Sistema de Software]<br/>Permite la compra y venta de productos para mascotas, gestión de inventario y despacho."]

    subgraph Externos ["☁️ Sistemas Externos"]
        PAS["💳 Pasarela de Pagos<br/>(Culqi / Niubiz / Yape)<br/>Procesa transacciones con tarjeta y billeteras."]
        ERP["🏢 ERP Empresarial<br/>Sincroniza inventarios globales y facturación."]
        LOG["🚚 Servicio Logístico / Envíos<br/>Gestiona el despacho y tracking de entregas."]
    end

    C -->|Realiza pedidos y pagos| SYS
    S -->|Gestiona catálogo y ventas| SYS
    A -->|Administra usuarios y reportes| SYS

    SYS -->|Procesa cobros| PAS
    SYS -->|Sincroniza stock y facturas| ERP
    SYS -->|Solicita guías de despacho| LOG

    style SYS fill:#1565c0,color:#fff,stroke:#0d47a1,stroke-width:3px
    style Usuarios fill:#e3f2fd,stroke:#1565c0,stroke-width:2px
    style Externos fill:#eceff1,stroke:#37474f,stroke-width:2px
```

---

## 3. Descripción de Actores y Sistemas

| Elemento | Tipo | Descripción |
| :--- | :--- | :--- |
| **Cliente** | Persona | Usuario final que explora el catálogo, agrega productos al carrito y realiza compras. |
| **Vendedor (Seller)** | Persona | Comercio registrado que gestiona sus productos, precios y solicitudes de pedido. |
| **Administrador** | Persona | Usuario interno encargado del monitoreo, soporte y gestión de permisos. |
| **Marketplace GoPet** | Sistema | Plataforma central que orquesta las operaciones de e-commerce. |
| **Pasarela de Pagos** | Sistema Externo | Servicio externo seguro encargado del cobro transaccional. |
| **ERP Empresarial** | Sistema Externo | Sistema de gestión empresarial para contabilidad e inventario consolidado. |
| **Servicio Logístico** | Sistema Externo | Empresa externa de envíos encargada de la entrega física del producto. |