graph TB
    BUS[("🐇 RabbitMQ Bus")]
    DB[("🗄️ PostgreSQL + PostGIS")]

    subgraph ServicioProcesamiento ["⚡ Servicio de Procesamiento de Incendios"]
        CONSUMER["📡 Event Consumer<br/>Escucha la cola 'puntos_calor_queue'"]
        VALIDATOR["🔍 Validador Geoespacial<br/>Filtra puntos dentro del territorio peruano"]
        EVALUATOR["🔥 Evaluador de Riesgo<br/>Aplica reglas de temperatura de brillo y confianza"]
        REPO["🗃️ Repositorio PostGIS<br/>Ejecuta transacciones georreferenciadas"]
        PRODUCER["📢 Alert Publisher<br/>Publica eventos en la cola 'alertas_queue'"]
    end

    BUS -->|Evento: PuntoCalorDetectado| CONSUMER
    CONSUMER --> VALIDATOR
    VALIDATOR --> EVALUATOR
    EVALUATOR --> REPO
    EVALUATOR --> PRODUCER
    REPO --> DB
    PRODUCER -->|Evento: AlertaGenerada| BUS

    style ServicioProcesamiento fill:#fbe9e7,stroke:#d84315,stroke-width:2px