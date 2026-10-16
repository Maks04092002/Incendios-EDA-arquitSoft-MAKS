# Restricciones del Sistema

| ID | Restricción | Descripción |
| :--- | :--- | :--- |
| **RC01** | **Paradigma Orientado a Eventos** | El sistema debe diseñarse e implementarse bajo la arquitectura orientada a eventos (EDA) utilizando un Message Broker distribuido (Apache Kafka o RabbitMQ). |
| **RC02** | **Procesamiento de Flujos (Stream Processing)** | El cálculo de algoritmos y correlación de riesgo debe realizarse mediante motores CEP como Apache Flink o Spark Streaming. |
| **RC03** | **Almacenamiento Geoespacial y Temporal** | La persistencia de capas vectoriales y series temporales debe usar PostgreSQL con extensiones PostGIS y TimescaleDB. |
| **RC04** | **Integración con Fuentes Externas Abiertas** | Debe consumir obligatoriamente los servicios públicos de NASA FIRMS (VIIRS/MODIS), Copernicus Sentinel y GOES-16. |
| **RC05** | **Entrega de Alertas en Tiempo Real** | La capa de presentación (WebGIS) debe recibir las actualizaciones de incendios mediante WebSockets para evitar el refresco manual. |