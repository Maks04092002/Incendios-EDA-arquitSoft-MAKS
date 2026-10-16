# Principios de Diseño SOLID en el Sistema de Monitoreo de Incendios

La aplicación de los principios **SOLID** en el diseño interno de los microservicios en Python/FastAPI garantiza que el núcleo de procesamiento de anomalías térmicas sea altamente mantenible, extensible y fácil de probar.

---

## 1. SRP - Single Responsibility Principle (Principio de Responsabilidad Única)
Cada módulo y clase tiene una sola razón de cambio dentro de la arquitectura EDA:
* **`FIRMSIngestionService`:** Responsable exclusivamente de realizar el polling HTTP a la API de NASA FIRMS y obtener los datos crudos.
* **`EvaluadorRiesgoIncendio`:** Encargado únicamente de calcular la temperatura de brillo y clasificar la severidad del foco de calor (Bajo, Medio, Alto, Crítico).
* **`PuntoCalorRepository`:** Encargado en forma exclusiva de ejecutar las consultas de persistencia espacial (`ST_MakePoint`, `ST_Contains`) en PostGIS.
* **`RabbitMQPublisher`:** Encargado únicamente de formatear y serializar mensajes GeoJSON hacia las colas del bus de eventos.

---

## 2. OCP - Open/Closed Principle (Principio de Abierto/Cerrado)
Los módulos están abiertos a la extensión pero cerrados a la modificación:
* Mediante la interfaz `IEstrategiaEvaluacionRiesgo`, es posible incorporar nuevas reglas de clasificación de incendios (ejemplo: incorporar índices NDVI de Copernicus o datos de vientos de SENAMHI) creando una nueva clase sin modificar el motor principal de procesamiento CEP.

---

## 3. LSP - Liskov Substitution Principle (Principio de Sustitución de Liskov)
Las implementaciones concretas de proveedores de datos satelitales cumplen un contrato uniforme:
* Las clases `NASAFirmsAdapter`, `CopernicusAdapter` y `GOES16Adapter` implementan la interfaz `IFuenteSatelital`. Cualquier componente consumidor puede tratar a cualquiera de ellas indistintamente mediante el método `obtenerAnomalias()`, garantizando la sustitución sin alterar la lógica de la aplicación.

---

## 4. ISP - Interface Segregation Principle (Principio de Segregación de Interfaces)
Se definen contratos específicos y enfocados en lugar de interfaces monolíticas:
* **`INotificadorAlertas`:** Define únicamente el método `enviarAlerta(alerta: AlertaRiesgo)`.
* **`IAnomaliaReader`:** Define únicamente operaciones de lectura de coordenadas y temperaturas de brillo.
* Ningún componente está obligado a depender de métodos que no utiliza.

---

## 5. DIP - Dependency Inversion Principle (Principio de Inversión de Dependencias)
Los casos de uso del dominio dependen de abstracciones, no de detalles tecnológicos concretos:
* El caso de uso `ProcesarPuntoCalorUseCase` depende de las interfaces abstractas `IPuntoCalorRepository` y `IEventBroker`, en lugar de acoplarse directamente a las librerías concretas `psycopg2` o `pika` (RabbitMQ). Esto permite realizar pruebas unitarias con repositorios *Mock* sin necesidad de levantar bases de datos reales.