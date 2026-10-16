graph TB
    subgraph Cliente ["🌐 Capa de Presentación WebGIS"]
        WEB["💻 Plataforma WebGIS<br/>[Contenedor: React + Leaflet]<br/>Visualización de mapas de puntos de calor y panel de alertas."]
    end

    subgraph Backend ["📦 Microservicios y Bus de Eventos"]
        INGEST["🐍 Servicio de Ingesta Satelital<br/>[Contenedor: Python / FastAPI]<br/>Consulta periódicamente NASA FIRMS y emite eventos."]
        
        BUS[("🐇 Bus de Eventos<br/>[Contenedor: RabbitMQ]<br/>Enruta eventos 'PuntoCalorDetectado' y 'AlertaGenerada'.")]
        
        PROC["⚡ Servicio de Procesamiento y Alertas<br/>[Contenedor: Python / FastAPI]<br/>Consume eventos, valida filtros espaciales y clasifica el riesgo."]
        
        API["🚀 API REST Gateway<br/>[Contenedor: Python / FastAPI]<br/>Expone endpoints para la plataforma WebGIS."]
    end

    subgraph Persistencia ["💾 Almacenamiento Geoespacial"]
        DB[("🗄️ Base de Datos Geoespacial<br/>[Contenedor: PostgreSQL + PostGIS]<br/>Guarda coordenadas, fechas, confianza y geometría de incendios.")]
    end

    EXT["🛰️ NASA FIRMS API"]

    INGEST -->|HTTPS GET| EXT
    INGEST -->|Publica evento 'PuntoCalorDetectado'| BUS
    BUS -->|Consume eventos de anomalías| PROC
    PROC -->|Publica 'AlertaGenerada' / Guarda datos| DB
    PROC -->|Notifica alerta| BUS

    WEB -->|HTTPS / REST API| API
    API -->|Consultas espaciales SQL/PostGIS| DB

    style BUS fill:#ff6f00,color:#fff,stroke:#e65100,stroke-width:2px
    style DB fill:#0277bd,color:#fff,stroke:#01579b,stroke-width:2px
    style PROC fill:#2e7d32,color:#fff,stroke:#1b5e20,stroke-width:2px
    style INGEST fill:#2e7d32,color:#fff,stroke:#1b5e20,stroke-width:2px