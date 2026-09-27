# Drivers Arquitectónicos

| ID | Driver Arquitectónico | Origen | ¿Por qué influye en la arquitectura? |
| :--- | :--- | :--- | :--- |
| **DA01** | Reducción drástica de latencia en la alerta de incendios. | AC01 – Rendimiento | Exige abandonar procesos por lotes (batch) e implementar un procesamiento continuo de flujos (Stream Processing). |
| **DA02** | Desacoplamiento total entre fuentes satelitales y consumidores. | RC01 – Event Broker | Obliga a usar un Message Broker con garantías de entrega (*at-least-once*) y particionamiento regional. |
| **DA03** | Ingesta masiva e ininterrumpida de fuentes heterogéneas. | AC03 – Resiliencia | Impone el uso de microservicios de ingesta asíncrona con búferes de tolerancia a fallos ante cortes de red. |
| **DA04** | Difusión automatizada y multicanal de alertas de peligro. | RF05 – Alertas | Requiere la creación de servicios consumidores dedicados a enviar notificaciones (Webhooks, SMS, Email) de forma asíncrona. |
| **DA05** | Almacenamiento optimizado de series de tiempo y datos GIS. | RC03 – Base de Datos | Condiciona el modelo de datos a bases de datos relacionales con soporte geoespacial (PostGIS) y series temporales (TimescaleDB). |