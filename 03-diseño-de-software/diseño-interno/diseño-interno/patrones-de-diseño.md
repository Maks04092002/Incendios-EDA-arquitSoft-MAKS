# Patrones de Diseño Implementados

Para resolver problemas recurrentes en el procesamiento de eventos térmicos y la integración con servicios externos, se aplican los siguientes patrones GoF y de integración de sistemas:

---

## 1. Patrón Adapter (Estructural)
* **Propósito:** Convertir la respuesta cruda de la API externa de NASA FIRMS (formato CSV/JSON) a las entidades de dominio internas en formato **GeoJSON**.
* **Aplicación:** La clase `NASAFirmsAdapter` transforma atributos como `bright_ti4`, `confidence` y `latitude/longitude` en objetos de dominio estandarizados `PuntoCalor`.

---

## 2. Patrón Strategy (Comportamiento)
* **Propósito:** Permitir algoritmos intercambiables de evaluación del nivel de riesgo de incendios.
* **Aplicación:** El motor de reglas utiliza `EstrategiaTemperaturaBrillo` para análisis térmico básico, y puede cambiar en tiempo de ejecución a `EstrategiaCorrelacionANP` cuando el foco de calor se encuentra dentro de un Área Natural Protegida.

---

## 3. Patrón Repository (Persistencia)
* **Propósito:** Encapsular la lógica de acceso a datos espacio-temporales aislándola del dominio.
* **Aplicación:** `PuntoCalorPostGISRepository` encapsula las sentencias SQL espaciales de PostGIS (`ST_SetSRID`, `ST_MakePoint`, `ST_DWithin`), ofreciendo métodos limpios como `guardar_foco(punto)` y `obtener_activos_por_region(poligono)`.

---

## 4. Patrón Publish / Subscribe (Integración EDA)
* **Propósito:** Desacoplar la producción de anomalías térmicas del consumo de alertas.
* **Aplicación:** El servicio de ingesta actúa como *Publisher* enviando el evento `PuntoCalorDetectado` al Exchange de RabbitMQ. Los microservicios de evaluación de riesgo, persistencia en PostGIS y notificaciones se suscriben independientemente como *Subscribers*.

---

## 5. Patrón Circuit Breaker (Resiliencia)
* **Propósito:** Prevenir caídas en cascada cuando los servicios de NASA FIRMS o los gateways de SMS experimenten interrupciones o alta latencia.
* **Aplicación:** Interrumpe temporalmente las peticiones hacia la API externa tras 3 fallos consecutivos, activando una cola de reintentos asíncronos con tiempo de espera exponencial (*Exponential Backoff*).