# Arquitectura Técnica Orientada a Eventos (EDA)

## 📐 Topología de Servidores y Diagrama de Arquitectura

```mermaid
flowchart TD

subgraph FUENTES ["🌐 FUENTES SATELITALES EXTERNAS"]
    NASA["🛰️ NASA FIRMS (VIIRS/MODIS)"]
    GOES["🛰️ NOAA GOES-16"]
    COP["🛰️ Copernicus Sentinel"]
end

subgraph S1 ["🖥️ SERVIDOR 1: BUS DE EVENTOS (BROKER)"]
    Ingesta["🐍 Microservicios de Ingesta"]
    Broker["⚡ Apache Kafka / RabbitMQ"]
end

subgraph S2 ["🖥️ SERVIDOR 2: PROCESAMIENTO EN TIEMPO REAL"]
    CEP["🧠 Engine CEP (Apache Flink / Spark)"]
    Riesgo["🔥 Algoritmo de Cálculo de Riesgo"]
end

subgraph S3 ["🖥️ SERVIDOR 3: BASE DE DATOS GEOESPACIAL"]
    BD[("🗄️ PostgreSQL + PostGIS + TimescaleDB")]
end

subgraph S4 ["🖥️ SERVIDOR 4: WEBGIS Y API GATEWAY"]
    API["🚪 API Gateway (Nginx / FastAPI)"]
    WebGIS["🗺️ Portal WebGIS (React + Mapbox)"]
    Alertas["🔔 Servicio de Notificaciones (WebSockets / SMS)"]
end

subgraph DESTINO ["🏛️ ENTIDADES Y BRIGADAS"]
    COEN["🚨 COEN / INDECI"]
    SERFOR["🌳 SERFOR / MINAM"]
    Brigadas["👨‍🚒 Brigadistas Locales"]
end

FUENTES --> Ingesta
Ingesta --> Broker
Broker --> CEP
CEP --> Riesgo
Riesgo --> BD
BD --> API
API --> WebGIS
Riesgo -. Alertas en tiempo real .-> Alertas
Alertas --> COEN
Alertas --> SERFOR
Alertas --> Brigadas

classDef fuenteStyle fill:#2D3748,stroke:#A0AEC0,color:#FFF,stroke-width:2px;
classDef s1Style fill:#1A365D,stroke:#3182CE,color:#FFF,stroke-width:2px;
classDef s2Style fill:#1C4532,stroke:#38A169,color:#FFF,stroke-width:2px;
classDef s3Style fill:#744210,stroke:#D69E2E,color:#FFF,stroke-width:2px;
classDef s4Style fill:#4A5568,stroke:#CBD5E0,color:#FFF,stroke-width:2px;
classDef destStyle fill:#2C5282,stroke:#63B3ED,color:#FFF,stroke-width:2px;

class NASA,GOES,COP fuenteStyle;
class Ingesta,Broker s1Style;
class CEP,Riesgo s2Style;
class BD s3Style;
class API,WebGIS,Alertas s4Style;
class COEN,SERFOR,Brigadas destStyle;
```

---

## 🖥️ Especificación de Servidores e Infraestructura

| Servidor | Rol Principal | Componentes Clave | Hardware Mínimo Recomendado |
| :--- | :--- | :--- | :--- |
| **Servidor 1** | **Bus de Eventos (Broker de Mensajes)** | Apache Kafka / RabbitMQ, Microservicios Python de Ingesta | 8 Núcleos (3.0 GHz+), 16 GB RAM, 500 GB SSD NVMe |
| **Servidor 2** | **Procesamiento en Tiempo Real (CEP)** | Apache Flink / Spark Streaming, Motor de Algoritmos | 8 Núcleos, 16 GB RAM, 256 GB SSD |
| **Servidor 3** | **Base de Datos Geoespacial** | PostgreSQL + PostGIS + TimescaleDB | 8 Núcleos, 32 GB RAM, 1 TB SSD NVMe |
| **Servidor 4** | **WebGIS y API Gateway** | Node.js, FastAPI, Nginx, React + Mapbox, WebSockets | 4 Núcleos (2.5 GHz+), 8 GB RAM, 128 GB SSD |

---

## ⚙️ Definición Operativa de Capas

1. **Capa de Ingesta:** Captura datos continuos en vivo desde NASA FIRMS, Copernicus y GOES-16 mediante llamadas asíncronas y los estandariza a formato GeoJSON[cite: 1].
2. **Capa de Mensajería y Eventos:** Garantiza el desacoplamiento total y la distribución continua de eventos clasificándolos por tópicos regionales (Norte, Centro, Sur, Oriente)[cite: 1].
3. **Capa de Análisis Stream:** Correlaciona anomalías térmicas (temperatura de brillo > 310 K) con datos meteorológicos y mapas de vegetación para calcular niveles de riesgo (Bajo, Medio, Alto, Crítico)[cite: 1].
4. **Capa de Persistencia:** Almacena coberturas geoespaciales y registros históricos en series temporales para análisis de tendencias[cite: 1].
5. **Capa de Presentación y Reacción:** Despliega el tablero WebGIS dinámico y despacha notificaciones automáticas instantáneas a autoridades (COEN/SERFOR) y brigadas de campo[cite: 1].