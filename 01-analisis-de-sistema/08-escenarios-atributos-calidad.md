# Escenarios de Calidad

Los escenarios son propuestas de medición. Todos permanecen como **NO
EJECUTADOS** hasta contar con la implementación, el entorno y la evidencia
correspondientes.

| ID | Atributo | Estímulo y entorno | Artefacto | Respuesta esperada | Medida y objetivo |
| :--- | :--- | :--- | :--- | :--- | :--- |
| EC-REN-01 | Rendimiento | 1000 observaciones simuladas en entorno local con RabbitMQ y un consumidor | Procesamiento | Consumir, validar, evaluar y confirmar eventos válidos | p95 desde recepción hasta confirmación; pendiente de línea base |
| EC-ESC-01 | Escalabilidad | Misma carga con 1, 2 y N consumidores | Broker y consumidores | Aumentar capacidad sin pérdida ni crecimiento sostenido de cola | throughput, cola, CPU y memoria; comparar contra línea base |
| EC-FIA-01 | Fiabilidad | Reinicio del consumidor y mensajes duplicados | Consumidor y repositorio | Reintentar, confirmar solo tras procesar e impedir duplicación | éxito, duplicados, recuperados y tiempo de recuperación |
| EC-DIS-01 | Disponibilidad | Interrupción controlada de API o consumidor | API y servicio de procesamiento | Recuperar el servicio y conservar el estado permitido | disponibilidad y MTTR; RTO/RPO pendientes de definir |
| EC-SEG-01 | Seguridad | Solicitudes con credenciales, roles y entradas válidas/ inválidas | API y administración | Autorizar solo operaciones permitidas y registrar accesos | porcentaje de autorizaciones correctas y rutas sensibles protegidas |
| EC-MAN-01 | Mantenibilidad | Sustituir un adaptador mediante un cambio controlado | Dominio y adaptadores | Mantener reglas de dominio y limitar pruebas afectadas | cobertura, complejidad, dependencias y pruebas afectadas |
| EC-INT-01 | Interoperabilidad | CSV/JSON válidos, inválidos y versiones de contrato | Adaptador y eventos | Normalizar válidos y rechazar inválidos con error trazable | registros válidos, mensajes conformes y errores de integración |
| EC-USA-01 | Usabilidad | Usuarios de prueba ejecutan consulta, filtro y lectura de alerta | WebGIS | Completar tareas sin ambigüedad crítica | tareas completadas, tiempo, errores y cuestionario definido |

Los procedimientos y plantillas de resultados están en
[medicion-atributos-calidad.md](medicion-atributos-calidad.md).
