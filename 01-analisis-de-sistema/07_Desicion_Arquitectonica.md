# Registro de Decisiones Arquitectónicas (ADRs)

El presente documento registra las decisiones de arquitectura adoptadas para responder a los drivers arquitectónicos y atributos de calidad definidos para el sistema de detección temprana de incendios forestales en el Perú.

| ID | Decisión arquitectónica | Driver relacionado | Justificación | Resultado |
| :--- | :--- | :--- | :--- | :--- |
| **ADR-001** | Arquitectura Dirigida por Eventos (EDA) con Bus de Mensajes | DA01 - Latencia <br> DA02 - Event Broker <br> DA03 - Resiliencia | Se propone desacoplar la ingesta, procesamiento, persistencia y notificación mediante eventos. | Seleccionar y configurar RabbitMQ como broker previsto; no está implementado en el repositorio actual. |
| **ADR-002** | Procesamiento de Observaciones en Flujo | DA01 - Latencia <br> DA03 - Resiliencia | Se propone procesar observaciones conforme estén disponibles, sin interpretar un punto de calor como incendio confirmado. | Definir un consumidor y reglas experimentales versionadas; la tecnología CEP queda pendiente de selección. |
| **ADR-003** | Persistencia Geoespacial | DA05 - Base de Datos GIS | Se propone conservar observaciones y evaluaciones para consultas territoriales e históricas. | PostgreSQL + PostGIS son tecnologías previstas; no están implementadas en el repositorio actual. |
| **ADR-004** | Clean Architecture e Integración por Adaptadores | DA04 - Alertas <br> DA06 - Mantenibilidad | Desacopla la lógica del cálculo de nivel de riesgo de los proveedores de notificaciones externas (Webhooks, SMS, WebSockets) y tecnologías de base de datos. | Módulos desacoplados en 4 capas (*Dominio, Aplicación, Adaptadores, Infraestructura*). |