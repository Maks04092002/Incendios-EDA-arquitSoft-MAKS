# Requisitos Funcionales

Los siguientes requisitos describen el alcance funcional previsto. La
implementación comprobada actualmente corresponde únicamente al boilerplate
Angular de marketplace; los requisitos satelitales permanecen como diseño
propuesto.

| ID | Descripción |
| :--- | :--- |
| **RF01** | El sistema debe recibir y normalizar observaciones de puntos de calor desde una fuente satelital integrada, inicialmente NASA FIRMS. |
| **RF02** | El sistema debe publicar observaciones normalizadas como eventos mediante el broker configurado. |
| **RF03** | El sistema debe validar la estructura y la ubicación de cada observación antes de procesarla. |
| **RF04** | El sistema debe ejecutar una evaluación experimental del riesgo con reglas versionadas y pendientes de validación científica. |
| **RF05** | El sistema debe correlacionar observaciones con capas territoriales disponibles, cuando esas fuentes sean integradas. |
| **RF06** | El sistema debe conservar observaciones, evaluaciones y alertas en almacenamiento geoespacial para consultas posteriores. |
| **RF07** | El sistema debe mostrar en el WebGIS los puntos de calor, capas disponibles y evaluaciones registradas. |
| **RF08** | El sistema debe generar y distribuir notificaciones cuando una regla configurada lo determine, sin prometer una latencia no medida. |
| **RF09** | El sistema debe permitir administrar usuarios, roles y parámetros operativos autorizados. |
| **RF10** | El sistema debe permitir consultar y exportar información histórica disponible. |

## Mapeo entre Historias de Usuario y Requisitos

| Historia | Requisitos relacionados |
| :--- | :--- |
| HU01 | RF06, RF07 |
| HU02 | RF04, RF08 |
| HU03 | RF05, RF07 |
| HU04 | RF09 |
| HU05 | RF06, RF10 |
| HU06 | RF06 |

## Reglas y límites de interpretación

- Un punto de calor es una detección u observación satelital; no es una
  confirmación de incendio.
- Los umbrales de temperatura, confianza y contexto territorial son reglas
  experimentales. Deben validarse con datos y criterios del dominio antes de
  usarse para decisiones operativas.
- El tiempo de observación depende de la disponibilidad y frecuencia de la
  fuente satelital.
- El tiempo de procesamiento interno comprende ingesta, validación,
  evaluación y persistencia.
- El tiempo de notificación comprende la publicación y la entrega al canal
  externo. Ninguno de estos tiempos está medido en el repositorio actual.
