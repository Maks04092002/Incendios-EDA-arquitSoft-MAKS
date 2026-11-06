# Atributos de Calidad

Estos atributos son objetivos de evaluación del sistema propuesto. El
repositorio todavía no contiene la integración NASA FIRMS, RabbitMQ,
PostgreSQL/PostGIS ni los servicios Python/FastAPI; por tanto, ningún valor de
rendimiento, disponibilidad o fiabilidad se considera medido.

| ID | Atributo | Enfoque de evaluación |
| :--- | :--- | :--- |
| AC01 | Rendimiento | Medir latencia de eventos, consultas REST, consultas geoespaciales y throughput bajo cargas controladas. |
| AC02 | Escalabilidad | Comparar uno y varios consumidores, observando throughput, colas y recursos sin afirmar escalabilidad antes de probarla. |
| AC03 | Fiabilidad | Verificar procesamiento correcto, duplicados, reintentos, confirmaciones e idempotencia ante fallos controlados. |
| AC04 | Disponibilidad y recuperación | Medir continuidad de API y consumidores, recuperación y MTTR en un entorno de pruebas. |
| AC05 | Seguridad | Verificar autenticación, autorización, secretos, entradas, rutas sensibles y vulnerabilidades. |
| AC06 | Mantenibilidad | Observar cobertura, complejidad, acoplamiento, reglas arquitectónicas y esfuerzo de cambio. |
| AC07 | Interoperabilidad | Validar formatos, contratos de eventos/API y errores de integración con fuentes y consumidores. |
| AC08 | Usabilidad del WebGIS | Medir tareas, tiempos, errores de interacción y valoración de usuarios de prueba. |

## Límites de interpretación

- La disponibilidad de NASA FIRMS no es la disponibilidad del sistema propio.
- La disponibilidad de una observación satelital no equivale al tiempo de
  procesamiento interno ni al tiempo de notificación.
- Un punto de calor es una observación; no representa un incendio confirmado.
- Los objetivos numéricos solo se convertirán en criterios de aceptación
  después de definir una línea base, recursos y un entorno reproducible.

La definición detallada de métricas, escenarios, fórmulas, pruebas y evidencias
se encuentra en [medicion-atributos-calidad.md](medicion-atributos-calidad.md).
