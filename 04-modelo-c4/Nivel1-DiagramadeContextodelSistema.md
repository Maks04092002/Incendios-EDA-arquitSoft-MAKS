# C1 — Diagrama de Contexto del Sistema

Vista de contexto: solo muestra personas, el sistema de monitoreo y sistemas
externos. RabbitMQ, bases de datos y clases pertenecen a niveles posteriores.

```mermaid
flowchart LR
    COEN["Analista COEN"]
    SERFOR["Gestor SERFOR"]
    INDECI["Analista INDECI"]
    Brigada["Brigadista local"]
    Admin["Administrador del sistema"]
    Sistema["Sistema de monitoreo de puntos de calor y evaluación experimental de riesgo"]
    FIRMS["NASA FIRMS"]

    COEN -->|Consulta monitoreo| Sistema
    SERFOR -->|Consulta contexto territorial| Sistema
    INDECI -->|Consulta y exporta información| Sistema
    Brigada -->|Recibe notificaciones| Sistema
    Admin -->|Administra acceso y configuración| Sistema
    FIRMS -->|Proporciona observaciones satelitales| Sistema
```

El sistema trabaja con observaciones satelitales y evaluaciones
experimentales. No confirma incendios por sí mismo.
