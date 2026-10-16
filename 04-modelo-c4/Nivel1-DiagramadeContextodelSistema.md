graph TB
    subgraph Actores ["👥 Actores del Sistema"]
        U["👤 Usuario de Monitoreo / Analista<br/>(Consulta puntos de calor y alertas)"]
        A["🛡️ Administrador del Sistema<br/>(Configura umbrales y supervisa servicios)"]
    end

    SYS["🔥 Sistema de Monitoreo de Incendios Forestales (EDA)<br/>[Sistema de Software]<br/>Recibe datos satelitales, procesa eventos de anomalías térmicas y genera alertas de riesgo."]

    subgraph Externos ["☁️ Fuentes y Sistemas Externos"]
        FIRMS["🛰️ NASA FIRMS API<br/>Proporciona registros de puntos de calor (MODIS / VIIRS)."]
        ENT["🏛️ Plataformas Institucionales (SERFOR / CENEPRED)<br/>Receptores potenciales de reportes de riesgo."]
    end

    U -->|Consulta mapa interactivo y alertas| SYS
    A -->|Administra usuarios y parámetros| SYS

    FIRMS -->|Ingesta de datos satelitales| SYS
    SYS -->|Exportación de datos de riesgo| ENT

    style SYS fill:#d84315,color:#fff,stroke:#bf360c,stroke-width:3px
    style Actores fill:#e3f2fd,stroke:#1565c0,stroke-width:2px
    style Externos fill:#eceff1,stroke:#37474f,stroke-width:2px