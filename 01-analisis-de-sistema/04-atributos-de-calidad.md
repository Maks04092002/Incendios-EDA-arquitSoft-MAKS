# Atributos de Calidad

| ID | Atributo de Calidad | Escenario / Requisito de Calidad |
| :--- | :--- | :--- |
| **AC01** | **Rendimiento y Baja Latencia** | El sistema debe procesar eventos satelitales y determinar niveles de riesgo en tiempo real con una latencia inferior a pocos segundos desde la ingesta. |
| **AC02** | **Escalabilidad Horizontal** | El bus de eventos (Kafka/RabbitMQ) debe poder escalar dinámicamente mediante particiones regionales para soportar picos masivos de focos de calor simultáneos durante la temporada seca. |
| **AC03** | **Disponibilidad y Resiliencia** | El sistema debe contar con mecanismos de reintento automático y búferes para operar de forma ininterrumpida ante caídas temporales de las APIs de proveedores satelitales. |
| **AC04** | **Seguridad** | Toda la comunicación hacia el WebGIS y API Gateway debe autenticarse mediante tokens JWT/OAuth2 con control de acceso basado en roles (SERFOR, COEN, Bomberos). |
| **AC05** | **Mantenibilidad y Desacoplamiento** | La arquitectura orientada a eventos (EDA) debe mantener un desacoplamiento estricto entre productores (ingesta satelital) y consumidores (procesador CEP, alertas, BD). |