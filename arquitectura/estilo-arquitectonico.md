# PASO 4: Estilo Arquitectónico Global

## Estilo Seleccionado: Arquitectura Dirigida por Eventos (EDA)

### Descripción
El sistema adopta una **Arquitectura Dirigida por Eventos (EDA)** para el procesamiento en tiempo real de datos satelitales (NASA FIRMS, GOES-16, Copernicus)[cite: 10, 13]. La comunicación entre componentes se realiza mediante la publicación y consumo de eventos asíncronos a través de un bus de mensajes (Apache Kafka / RabbitMQ), garantizando un desacoplamiento total y baja latencia en la detección de incendios forestales en el Perú[cite: 12, 13].

---

## Diagrama de Arquitectura Global

```mermaid
graph TD
    subgraph Fuentes_Satelitales [Fuentes Satelitales Externas]
        FIRMS[NASA FIRMS / VIIRS]
        GOES[GOES-16]
        COP[Copernicus Sentinel]
    end

    subgraph Ingesta [Capa de Ingesta Asíncrona]
        I1[Ingestor FIRMS]
        I2[Ingestor GOES]
    end

    subgraph Broker [Bus de Eventos - Message Broker]
        KAFKA[Apache Kafka / RabbitMQ]
    end

    subgraph CEP [Procesamiento en Tiempo Real]
        FLINK[Apache Flink - Motor CEP]
    end

    subgraph Persistencia [Base de Datos Espacio-Temporal]
        DB[(PostgreSQL + PostGIS + TimescaleDB)]
    end

    subgraph Presentacion [Capa de Presentación y Servicios]
        API[API Gateway / Backend Node.js]
        WEBGIS[Portal WebGIS / Mapbox]
        NOTIF[Servicio de Alertas]
    end

    subgraph Actores [Destinatarios de Alertas]
        COEN[COEN / INDECI]
        SERFOR[SERFOR / Brigadas]
    end

    FIRMS --> I1
    GOES --> I2
    COP --> I1

    I1 -->|Eventos GeoJSON| KAFKA
    I2 -->|Eventos GeoJSON| KAFKA

    KAFKA -->|Flujo continuo| FLINK
    FLINK -->|Persistencia de focos| DB
    FLINK -->|Eventos de riesgo crítico| NOTIF

    DB -->|Consultas GIS| API
    API --> WEBGIS
    NOTIF -->|Webhooks / SMS| COEN
    NOTIF -->|Push / Email| SERFOR