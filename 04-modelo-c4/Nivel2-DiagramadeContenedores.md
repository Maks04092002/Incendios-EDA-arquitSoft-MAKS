# C2 — Diagrama de Contenedores

En C4, un contenedor es una unidad de ejecución o almacenamiento relevante
para la arquitectura; no equivale necesariamente a un contenedor Docker.
Todos los elementos backend de esta vista son servicios propuestos.

```mermaid
flowchart LR
    FIRMS["NASA FIRMS<br/>Sistema externo"]
    WEB["WebGIS<br/>React + Leaflet<br/>Consulta y visualización"]
    ING["Servicio de Ingesta<br/>Python + FastAPI<br/>Adapta y publica observaciones"]
    PROC["Servicio de Procesamiento<br/>Python + FastAPI<br/>Valida y evalúa riesgo experimental"]
    STORE["Servicio de Persistencia<br/>Consumidor de eventos<br/>Persiste observaciones y resultados"]
    ALERT["Servicio de Alertas<br/>Consumidor de eventos<br/>Coordina notificaciones"]
    API["API de Consulta<br/>FastAPI<br/>Expone consultas protegidas"]
    ADMIN["API de Administración<br/>FastAPI<br/>Gestiona usuarios y configuración"]
    MQ[("RabbitMQ<br/>Broker de eventos")]
    DB[("PostgreSQL + PostGIS<br/>Persistencia geoespacial")]
    EXT["Canales externos<br/>SMS / Email / Webhook"]

    ING -->|HTTPS: consulta observaciones| FIRMS
    ING -->|AMQP: PuntoCalorDetectado| MQ
    MQ -->|AMQP: observaciones| PROC
    MQ -->|AMQP: observaciones y evaluaciones| STORE
    PROC -->|AMQP: EvaluacionRiesgoCalculada / AlertaGenerada| MQ
    MQ -->|AMQP: AlertaGenerada| ALERT
    STORE -->|SQL/PostGIS: persistencia| DB
    API -->|SQL/PostGIS: consultas| DB
    WEB -->|HTTPS/REST: consultas| API
    ADMIN -->|HTTPS/REST: administración| DB
    ALERT -->|API del proveedor: entrega solicitada| EXT
```

La relación entre RabbitMQ y PostgreSQL siempre pasa por el Servicio de
Persistencia, que consume eventos y ejecuta el repositorio. La disponibilidad
de estas tecnologías queda pendiente de implementación.

## Relación con los módulos funcionales

| Contenedor C4 | Módulo o módulos atendidos |
| :--- | :--- |
| Servicio de Ingesta | Gestión de Datos Satelitales |
| Servicio de Procesamiento | Gestión de Puntos de Calor; Evaluación Experimental del Riesgo |
| Servicio de Persistencia | Gestión de Puntos de Calor |
| API de Consulta + WebGIS | Monitoreo Geoespacial |
| Servicio de Alertas | Gestión de Alertas |
| API de Administración | Administración del Sistema |
