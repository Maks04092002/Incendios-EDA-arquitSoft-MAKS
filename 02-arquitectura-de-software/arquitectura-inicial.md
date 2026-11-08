# Arquitectura Lógica Inicial del Sistema

## 1. Descripción de la Arquitectura Base

Como punto de partida para el desarrollo incremental del prototipo, se definió una **Arquitectura Lógica en Tres Capas**, cuyo objetivo es establecer una separación clara de responsabilidades básicas antes de incorporar la comunicación asíncrona distribuida.

```mermaid
flowchart TB
    USR["Usuario de Monitoreo"]
    ADM["Administrador"]
    WEB["Cliente Web<br/>Navegador · React · Leaflet"]

    USR --> WEB
    ADM --> WEB

    subgraph PRESENTACION["1. CAPA DE PRESENTACIÓN"]
        direction LR
        API["API REST<br/>FastAPI"]
        M1P["Gestión de Datos<br/>Satelitales"]
        M2P["Gestión de<br/>Puntos de Calor"]
        M3P["Evaluación<br/>del Riesgo"]
        M4P["Monitoreo<br/>Geoespacial"]
        M5P["Gestión de<br/>Alertas"]
        M6P["Administración<br/>del Sistema"]
    end

    subgraph NEGOCIO["2. CAPA DE LÓGICA DE NEGOCIO"]
        direction LR
        M1S["Ingesta, validación<br/>y normalización"]
        M2S["Registro,<br/>deduplicación y consulta"]
        M3S["Clasificación<br/>experimental"]
        M4S["Mapas, filtros<br/>e historial"]
        M5S["Generación y<br/>distribución"]
        M6S["Usuarios, roles<br/>y auditoría"]
    end

    subgraph INFRAESTRUCTURA["3. CAPA DE DATOS E INFRAESTRUCTURA"]
        direction LR
        FIRMS["NASA FIRMS<br/>Fuente satelital"]
        R1["Adaptador<br/>NASA FIRMS"]
        R2["Repositorio<br/>Puntos de Calor"]
        R3["Repositorio<br/>Evaluaciones"]
        R4["Repositorio<br/>Monitoreo"]
        R5["Repositorio<br/>Alertas"]
        R6["Repositorio<br/>Administración"]
        DB[("PostgreSQL + PostGIS<br/>monitoreo_db")]
        CANALES["Canales de notificación<br/>Email · SMS · Webhook"]
    end

    WEB -->|"HTTPS / JSON"| API
    API --> M1P
    API --> M2P
    API --> M3P
    API --> M4P
    API --> M5P
    API --> M6P

    M1P --> M1S --> R1
    M2P --> M2S --> R2
    M3P --> M3S --> R3
    M4P --> M4S --> R4
    M5P --> M5S --> R5
    M6P --> M6S --> R6

    FIRMS -->|"HTTPS / CSV / JSON"| R1
    R2 --> DB
    R3 --> DB
    R4 --> DB
    R5 --> DB
    R6 --> DB
    M5S -->|"Notificación configurada"| CANALES

    classDef actor fill:#f8fafc,stroke:#475569,color:#0f172a,stroke-width:2px
    classDef cliente fill:#eef2ff,stroke:#4f46e5,color:#312e81,stroke-width:2px
    classDef presentacion fill:#dbeafe,stroke:#2563eb,color:#1e3a8a,stroke-width:2px
    classDef negocio fill:#dcfce7,stroke:#16a34a,color:#14532d,stroke-width:2px
    classDef infraestructura fill:#fef3c7,stroke:#d97706,color:#92400e,stroke-width:2px
    classDef externo fill:#ede9fe,stroke:#7c3aed,color:#4c1d95,stroke-width:2px
    classDef almacenamiento fill:#e2e8f0,stroke:#475569,color:#0f172a,stroke-width:2px

    class USR,ADM actor
    class WEB cliente
    class API,M1P,M2P,M3P,M4P,M5P,M6P presentacion
    class M1S,M2S,M3S,M4S,M5S,M6S negocio
    class R1,R2,R3,R4,R5,R6 infraestructura
    class FIRMS,CANALES externo
    class DB almacenamiento

    style PRESENTACION fill:#eff6ff,stroke:#2563eb,stroke-width:3px
    style NEGOCIO fill:#f0fdf4,stroke:#16a34a,stroke-width:3px
    style INFRAESTRUCTURA fill:#fffbeb,stroke:#d97706,stroke-width:3px
```


---

## 2. Detalle de las Capas Iniciales

### 2.1. Capa de Presentación (WebGIS)
* **Tecnologías:** React.js, Leaflet, Tailwind CSS.
* **Responsabilidad:** Renderizar mapas con marcadores térmicos, mapas de calor (*heatmaps*), filtros por fecha/región y panel de control de alertas activas para los analistas.

### 2.2. Capa de Lógica de Negocio
* **Tecnologías previstas:** Python y FastAPI. No están presentes en el código actual.
* **Responsabilidad:**
  * Validar las coordenadas geográficas (latitud, longitud) y normalizar el formato de temperatura de brillo (Kelvin/Celsius).
  * Evaluar si la anomalía térmica cae dentro del polígono del territorio peruano.
  * Aplicar la matriz inicial de clasificación de riesgo (Bajo, Medio, Alto, Crítico).

### 2.3. Capa de Acceso a Datos e Integración
* **Tecnologías previstas:** PostgreSQL, PostGIS y un adaptador de persistencia. No están presentes en el código actual.
* **Responsabilidad:**
  * Conectarse a la API de NASA FIRMS para la ingesta periódica de registros.
  * Almacenar datos vectoriales en tablas espaciales (`GEOMETRY(Point, 4326)`).
  * Ejecutar consultas geométricas complejas (`ST_Contains`, `ST_Buffer`, `ST_DWithin`).

---

## 3. Justificación de la Transición hacia la Arquitectura EDA

Aunque la arquitectura en tres capas inicial permitió estructurar el prototipo de forma comprensible, presentó las siguientes **limitaciones técnicas** ante el escenario real de incendios forestales:

1. **Acoplamiento Sincrónico:** Si la API de NASA FIRMS experimenta alta latencia o caídas, la interfaz de usuario se bloquea esperando la respuesta.
2. **Cuello de Botella en Lectura/Escritura:** El procesamiento por lotes (*batch*) directo contra la base de datos ralentiza las consultas de mapas en tiempo real durante emergencias.
3. **Falta de Escalabilidad Horizontal:** Imposibilidad de escalar únicamente el componente de alertas sin duplicar todo el servidor backend.

Por ello, esta arquitectura inicial evoluciona hacia una **Arquitectura Orientada a Eventos (EDA)** con microservicios desacoplados.