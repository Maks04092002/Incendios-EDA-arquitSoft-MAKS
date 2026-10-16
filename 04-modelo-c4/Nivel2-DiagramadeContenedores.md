# C4 Nivel 2: Diagrama de Contenedores (Containers)

## 1. Descripción General
El Diagrama de Contenedores muestra la arquitectura de software de alto nivel, identificando las aplicaciones, bases de datos y almacenes de datos que componen el sistema GoPet.

---

## 2. Diagrama de Contenedores

```mermaid
graph TB
    subgraph Usuarios ["👥 Usuarios"]
        U["📱 Cliente / Seller / Admin"]
    end

    subgraph GoPet ["📦 Sistema Marketplace GoPet"]
        WEB["💻 Aplicación Web<br/>[Contenedor: Next.js / React]<br/>Interfáz gráfica responsiva para usuarios."]
        
        API["🚀 API Backend<br/>[Contenedor: Node.js / Express]<br/>Proporciona la lógica de negocio vía REST API."]
        
        DB[("🗄️ Base de Datos Relacional<br/>[Contenedor: PostgreSQL]<br/>Almacena usuarios, productos, pedidos y pagos.")]
        
        CACHE[("⚡ Caché e Inventario Temporal<br/>[Contenedor: Redis]<br/>Almacena carritos de compra y caché de catálogo.")]
    end

    subgraph Externos ["☁️ Sistemas Externos"]
        PAS["💳 Pasarela de Pagos"]
        ERP["🏢 ERP Empresarial"]
        LOG["🚚 Servicio de Envío"]
    end

    U -->|HTTPS / HTML / JS| WEB
    WEB -->|JSON / HTTPS REST| API

    API -->|Consultas SQL| DB
    API -->|Lectura / Escritura rápida| CACHE

    API -->|HTTPS / REST| PAS
    API -->|HTTPS / REST| ERP
    API -->|HTTPS / REST| LOG

    style WEB fill:#7b1fa2,color:#fff,stroke:#4a148c
    style API fill:#2e7d32,color:#fff,stroke:#1b5e20
    style DB fill:#e65100,color:#fff,stroke:#bf360c
    style CACHE fill:#e65100,color:#fff,stroke:#bf360c
```

---

## 3. Especificación de Contenedores

| Contenedor | Tecnología | Responsabilidad |
| :--- | :--- | :--- |
| **Aplicación Web** | Next.js / React, Tailwind | Interfaz de usuario para navegación, carrito y panel de administración. |
| **API Backend** | Node.js, Express, TypeScript | Procesa peticiones HTTP, aplica reglas de negocio y coordina módulos. |
| **Base de Datos** | PostgreSQL | Persistencia relacional con integridad transaccional ACID. |
| **Caché** | Redis | Almacenamiento en memoria para sesiones, carritos activos y catálogo frecuente. |