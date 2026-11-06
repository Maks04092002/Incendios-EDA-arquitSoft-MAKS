# Estilo Arquitectónico

## EDA como estilo principal

La Arquitectura Orientada a Eventos (EDA) es el estilo principal propuesto.
Los productores publican eventos y los consumidores reaccionan de forma
asíncrona mediante un broker. El diseño no implica que todos los flujos sean
asíncronos: las consultas del WebGIS pueden usar HTTP/REST hacia una API.

Eventos propuestos:

- `PuntoCalorDetectado`: observación normalizada, no incendio confirmado.
- `EvaluacionRiesgoCalculada`: resultado experimental versionado.
- `AlertaGenerada`: resultado que cumple una regla de notificación configurada.

## Arquitectura inicial en capas

La arquitectura inicial en capas sirve como modelo de separación para el
prototipo:

1. **Presentación:** WebGIS y API de consulta.
2. **Aplicación y dominio:** casos de uso, validación y evaluación experimental.
3. **Adaptadores e infraestructura:** fuentes externas, broker y persistencia.

La evolución a EDA desacopla los flujos de ingesta, procesamiento,
persistencia y notificación; no elimina la separación de responsabilidades.

## Diagrama del estilo EDA propuesto

El siguiente diagrama muestra la comunicación asíncrona prevista y la
separación entre servicios, broker y persistencia. Es un diseño propuesto; no
representa una implementación existente.

```mermaid
flowchart LR
    FIRMS["NASA FIRMS<br/>Sistema externo"]
    ING["Servicio de Ingesta<br/>Adaptador de fuente"]
    BROKER[("RabbitMQ<br/>Broker de eventos")]
    PROC["Servicio de Procesamiento<br/>Validación y evaluación experimental"]
    STORE["Servicio de Persistencia<br/>Consumidor de eventos"]
    ALERT["Servicio de Alertas<br/>Consumidor de eventos"]
    API["API de Consulta"]
    WEB["WebGIS<br/>React + Leaflet"]
    DB[("PostgreSQL + PostGIS")]
    CANAL["Canales externos<br/>SMS / Email / Webhook"]

    ING -->|Consulta| FIRMS
    ING -->|Publica PuntoCalorDetectado| BROKER
    BROKER -->|Entrega observación| PROC
    BROKER -->|Entrega observación y evaluación| STORE
    PROC -->|Publica EvaluacionRiesgoCalculada| BROKER
    PROC -->|Publica AlertaGenerada si corresponde| BROKER
    BROKER -->|Entrega AlertaGenerada| ALERT
    STORE -->|Persiste mediante repositorio| DB
    API -->|Consulta datos geoespaciales| DB
    WEB -->|HTTP/REST| API
    ALERT -->|Solicita entrega| CANAL
```

RabbitMQ no persiste directamente en PostgreSQL. El Servicio de Persistencia
consume los eventos y utiliza un repositorio para guardar la información.

## Clean Architecture y arquitectura hexagonal

El dominio y los casos de uso dependen de puertos, no de FastAPI, RabbitMQ,
PostgreSQL o SDK de una fuente. Los adaptadores de entrada incluyen
consumidores de eventos y endpoints; los adaptadores de salida incluyen el
publicador, repositorio y clientes externos.

Esta organización es un diseño propuesto. El código real inspeccionado es un
boilerplate Angular de marketplace y no contiene estos servicios.

## Tecnologías previstas

| Área | Tecnología prevista | Estado comprobado |
| :--- | :--- | :--- |
| Fuente | NASA FIRMS | No integrada en el código actual |
| Backend | Python, FastAPI | No presente en el código actual |
| Mensajería | RabbitMQ | No presente en el código actual |
| Persistencia | PostgreSQL, PostGIS | No presente en el código actual |
| Presentación | React, Leaflet | No presente; el boilerplate usa Angular |

## Principios y patrones

- **SOLID:** guía para separar casos de uso, puertos y adaptadores; no prueba
  que las clases propuestas ya existan.
- **Adapter:** convierte el contrato de NASA FIRMS al modelo interno.
- **Strategy:** permite comparar reglas experimentales de evaluación.
- **Repository:** encapsula persistencia y consultas geoespaciales.
- **Publish/Subscribe:** desacopla productores y consumidores mediante eventos.

Estos patrones son decisiones de diseño propuestas, no implementaciones
verificadas en el sistema de monitoreo.
