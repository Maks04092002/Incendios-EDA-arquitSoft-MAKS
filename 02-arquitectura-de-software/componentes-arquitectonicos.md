# Componentes Arquitectónicos del Sistema

## 1. Visión General

El sistema de monitoreo de puntos de calor e incendios forestales está organizado en **componentes desacoplados** basados en una **Arquitectura Orientada a Eventos (EDA)** y **Microservicios**. Cada componente cumple una función especializada dentro del ciclo de vida del dato satelital: desde la ingesta hasta la visualización geográfica y notificación de alertas.

---

## 2. Desglose de Componentes Principales

### 2.1. Productores de Eventos (Event Producers)
* **`ms-ingesta-satelital` (Microservicio de Ingesta Satelital):**
  * **Tecnología:** Python 3.11, FastAPI, Celery / APScheduler.
  * **Función:** Realiza consultas periódicas (*polling*) a las APIs de **NASA FIRMS (VIIRS/MODIS)**, **Copernicus** y **GOES-16**. Estandariza la información recibida al formato **GeoJSON** y publica el evento `PuntoCalorDetectado`.

### 2.2. Bus de Eventos (Message Broker)
* **`broker-eventos` (RabbitMQ / Apache Kafka):**
  * **Tecnología:** RabbitMQ 3.12 / Apache Kafka.
  * **Función:** Actúa como el núcleo de comunicación asíncrona distribuida. Administra intercambios (*exchanges*), tópicos y colas particionadas por regiones (**Norte, Centro, Sur, Oriente**) para garantizar baja latencia y tolerancia a fallos.

### 2.3. Procesadores y Consumidores de Eventos (Event Processors / Consumers)
* **`ms-procesador-cep` (Motor de Procesamiento en Flujo CEP):**
  * **Tecnología:** Python (FastAPI / Flink / RxPY).
  * **Función:** Consume los eventos de anomalías térmicas en flujo continuo. Evalúa si la temperatura de brillo supera los **310 K**, realiza la geofocalización sobre el territorio peruano y calcula el nivel de riesgo (**Bajo, Medio, Alto, Crítico**). Emite el evento `AlertaGenerada`.

* **`ms-persistencia-gis` (Servicio de Persistencia Geoespacial):**
  * **Tecnología:** Python, GeoAlchemy2, PostGIS Driver.
  * **Función:** Consume asíncronamente los eventos de puntos de calor y alertas para almacenarlos de forma permanente en la base de datos espacio-temporal.

* **`ms-notificaciones-alertas` (Servicio de Alertas Multicanal):**
  * **Tecnología:** Node.js / Python.
  * **Función:** Escucha el evento `AlertaGenerada` (riesgo Alto o Crítico) y despacha notificaciones automáticas multicanal vía **SMS**, **Email**, **Webhooks** (para SERFOR/INDECI) y **WebSockets** (hacia la plataforma web).

### 2.4. Capa de Exposición y Presentación (API & WebGIS)
* **`ms-api-gateway` (API Gateway & Servidor WebSocket):**
  * **Tecnología:** Python / FastAPI, WebSockets, JWT.
  * **Función:** Expone endpoints REST para la consulta de datos históricos, filtrado por fecha/región y provee un canal de comunicación en tiempo real con la plataforma WebGIS.

* **`plataforma-webgis` (Frontend WebGIS):**
  * **Tecnología:** React.js, Leaflet / Mapbox, Tailwind CSS.
  * **Función:** Interfaz interactiva para analistas del COEN, INDECI y SERFOR. Muestra marcadores térmicos en vivo, mapas de calor (*heatmaps*), capas de cobertura vegetal/ANP y panel de alertas.

### 2.5. Persistencia Espacio-Temporal
* **`base-datos-postgis` (Base de Datos Espacial y Series de Tiempo):**
  * **Tecnología:** PostgreSQL 16 + PostGIS 3.4 + TimescaleDB.
  * **Función:** Almacena geometrías vectoriales (`GEOMETRY(Point, 4326)`), límites de Áreas Naturales Protegidas (ANP) y series temporales históricas de focos térmicos.

---
## 3. Matriz de Interacción entre Componentes

| Componente Origen | Componente Destino | Protocolo / Mecanismo | Formato / Evento | Propósito |
| :--- | :--- | :--- | :--- | :--- |
| **ms-ingesta-satelital** | **broker-eventos** | AMQP / TCP | GeoJSON (`PuntoCalorDetectado`) | Publicar anomalía térmica detectada en tiempo real. |
| **broker-eventos** | **ms-procesador-cep** | AMQP Push | GeoJSON | Enviar flujo de puntos de calor para evaluación de riesgo. |
| **ms-procesador-cep** | **broker-eventos** | AMQP / TCP | JSON (`AlertaGenerada`) | Emitir alerta clasificada (Riesgo Alto o Crítico). |
| **broker-eventos** | **ms-notificaciones-alertas** | AMQP Push | JSON | Despachar mensajes SMS/Webhooks a brigadistas y COEN. |
| **ms-persistencia-gis** | **base-datos-postgis** | SQL / PostGIS | SQL INSERT / PostGIS WKT | Persistir coordenadas, polígonos y métricas térmicas. |
| **ms-api-gateway** | **plataforma-webgis** | WebSockets / REST | GeoJSON / JSON | Actualizar el mapa de calor en vivo sin refresco manual. |

## 4. Diagrama de Componentes Arquitectónicos

```mermaid
graph TB
    subgraph Ingesta ["📡 Capa de Ingesta Satelital"]
        FIRMS["🛰️ NASA FIRMS API"]
        GOES["🛰️ NOAA GOES-16"]
        INGEST["🐍 ms-ingesta-satelital"]
        FIRMS & GOES -->|HTTP REST| INGEST
    end

    subgraph Messaging ["🐇 Bus de Eventos Distribuido"]
        BROKER[("RabbitMQ / Kafka Broker<br/>Colas: Norte, Centro, Sur, Oriente")]
    end

    subgraph Processing ["⚡ Capa de Microservicios Consumidores"]
        CEP["🔥 ms-procesador-cep<br/>(Reglas de Riesgo y CEP)"]
        STORE["💾 ms-persistencia-gis<br/>(Almacenamiento GIS)"]
        NOTIF["🔔 ms-notificaciones-alertas<br/>(SMS, Webhooks, Push)"]
    end

    subgraph Data ["🗄️ Persistencia"]
        DB[("PostgreSQL + PostGIS + TimescaleDB")]
    end

    subgraph Presentacion ["🌐 Capa de Presentación WebGIS"]
        GATEWAY["🚀 ms-api-gateway (FastAPI)"]
        WEBGIS["💻 plataforma-webgis (React + Leaflet)"]
    end

    INGEST -->|Publica: PuntoCalorDetectado| BROKER
    BROKER -->|Consumo en flujo| CEP
    BROKER -->|Consumo asíncrono| STORE
    CEP -->|Publica: AlertaGenerada| BROKER
    BROKER -->|Consumo de alertas| NOTIF

    STORE --> DB
    GATEWAY -->|SQL / Spatial Queries| DB
    NOTIF -.->|WebSockets| WEBGIS
    WEBGIS -->|HTTPS REST| GATEWAY

    style Messaging fill:#ff6f00,color:#fff,stroke:#e65100,stroke-width:2px
    style Processing fill:#2e7d32,color:#fff,stroke:#1b5e20,stroke-width:2px
    style Ingesta fill:#0277bd,color:#fff,stroke:#01579b,stroke-width:2px
    style Presentacion fill:#7b1fa2,color:#fff,stroke:#4a148c,stroke-width:2px

