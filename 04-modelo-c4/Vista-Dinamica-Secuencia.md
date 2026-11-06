# Vista dinámica — Procesamiento de una observación

Esta secuencia complementa los niveles C4; no sustituye al C4 de código.
Representa un flujo propuesto y separa observación, procesamiento y
notificación.

```mermaid
sequenceDiagram
    participant FIRMS as NASA FIRMS
    participant Ingesta as Servicio de Ingesta
    participant Broker as RabbitMQ
    participant Proc as Servicio de Procesamiento
    participant Persist as Servicio de Persistencia
    participant DB as PostgreSQL/PostGIS
    participant Alertas as Servicio de Alertas
    participant Canal as Canal externo

    Ingesta->>FIRMS: Solicitar observaciones disponibles
    FIRMS-->>Ingesta: Registros satelitales
    Ingesta->>Broker: Publicar PuntoCalorDetectado
    Broker-->>Proc: Entregar observación
    Proc->>Proc: Validar y evaluar experimentalmente
    Proc->>Broker: Publicar EvaluacionRiesgoCalculada
    Broker-->>Persist: Entregar evento
    Persist->>DB: Guardar observación y evaluación
    opt Regla de notificación cumplida
        Proc->>Broker: Publicar AlertaGenerada
        Broker-->>Alertas: Entregar alerta
        Alertas->>Canal: Solicitar entrega
    end
```

Los tiempos de disponibilidad satelital, procesamiento y entrega deben medirse
por separado. La secuencia no demuestra latencia ni entrega instantánea.
