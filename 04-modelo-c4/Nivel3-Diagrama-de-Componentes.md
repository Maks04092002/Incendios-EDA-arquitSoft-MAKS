# C3 — Componentes del Servicio de Procesamiento

Diseño propuesto del servicio que consume observaciones y calcula una
evaluación experimental. No corresponde a clases existentes en el boilerplate
actual.

```mermaid
flowchart LR
    MQ[("RabbitMQ")]
    subgraph Servicio["Servicio de Procesamiento"]
        Consumer["Consumidor de eventos<br/>Adaptador de entrada"]
        UseCase["Caso de uso<br/>ProcesarPuntoCalor"]
        Validator["Validador<br/>Estructura y coordenadas"]
        Evaluator["Evaluador experimental<br/>Strategy de reglas"]
        RepoPort["IPuntoCalorRepository<br/>Puerto de salida"]
        PublisherPort["IEventPublisher<br/>Puerto de salida"]
        Repo["Adaptador Repository PostGIS"]
        Publisher["Adaptador Publisher RabbitMQ"]
    end
    DB[("PostgreSQL + PostGIS")]

    MQ -->|PuntoCalorDetectado| Consumer
    Consumer -->|DTO normalizado| UseCase
    UseCase --> Validator
    Validator --> Evaluator
    Evaluator -->|EvaluacionRiesgoCalculada| UseCase
    UseCase --> RepoPort
    RepoPort -.implementa.-> Repo
    Repo -->|SQL/PostGIS| DB
    UseCase --> PublisherPort
    PublisherPort -.implementa.-> Publisher
    Publisher -->|AlertaGenerada, si corresponde| MQ
```

El caso de uso depende de interfaces. Las tecnologías solo aparecen en los
adaptadores.
