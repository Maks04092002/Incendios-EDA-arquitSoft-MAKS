# C4 — Nivel de Código

## Diseño propuesto: componente `ProcesarPuntoCalor`

Este nivel representa clases e interfaces de un componente concreto. Los
tipos son diseño propuesto: no existen todavía en el código real del
repositorio.

```mermaid
classDiagram
    class ConsumidorEventos {
        +recibir(evento)
    }
    class ProcesarPuntoCalor {
        +ejecutar(evento)
    }
    class ValidadorPuntoCalor {
        +validar(evento) ResultadoValidacion
    }
    class EvaluadorRiesgo {
        +evaluar(punto) EvaluacionRiesgo
    }
    class EstrategiaEvaluacion {
        <<interface>>
        +evaluar(punto) NivelRiesgoExperimental
    }
    class PuntoCalorRepository {
        <<interface>>
        +guardar(punto)
    }
    class EventPublisher {
        <<interface>>
        +publicar(evento)
    }
    class EstrategiaBasica {
        +evaluar(punto) NivelRiesgoExperimental
    }

    ConsumidorEventos --> ProcesarPuntoCalor
    ProcesarPuntoCalor --> ValidadorPuntoCalor
    ProcesarPuntoCalor --> EvaluadorRiesgo
    ProcesarPuntoCalor --> PuntoCalorRepository
    ProcesarPuntoCalor --> EventPublisher
    EvaluadorRiesgo --> EstrategiaEvaluacion
    EstrategiaEvaluacion <|.. EstrategiaBasica
```

Las reglas de `EstrategiaBasica` deben validarse experimentalmente. No se
define aquí un umbral térmico como confirmación de incendio.
