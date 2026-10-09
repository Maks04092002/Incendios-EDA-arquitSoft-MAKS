# PASO 5: Enfoque Arquitectónico Interno

## Resumen del Enfoque

| Elemento | Descripción aplicada al Sistema de Detección de Incendios |
| :--- | :--- |
| **Patrón / Enfoque arquitectónico** | Clean Architecture (Arquitectura Limpia). |
| **Objetivo** | Separar responsabilidades y controlar las dependencias hacia el dominio (las reglas del negocio de detección de incendios). |
| **¿Qué problema resuelve?** | Evita el acoplamiento directo entre los conectores satelitales (FIRMS/GOES-16), el Bus de Eventos (Kafka), la base de datos espacio-temporal (PostgreSQL/PostGIS) y los servicios de alerta/WebGIS. |
| **Capas definidas** | Infraestructura, Adaptadores (Interface Adapters), Aplicación (Casos de Uso) y Dominio. |
| **Beneficios** | • Facilita el mantenimiento evolutivo del sistema (DA06)[cite: 6, 8].<br>• Permite cambiar los proveedores de datos satelitales o el Message Broker sin modificar las reglas de cálculo de riesgo.<br>• Habilita la ejecución de pruebas unitarias sobre los algoritmos de detección sin depender de bases de datos ni redes activas. |

---

## Estructura de Capas e Inversión de Dependencias

Las dependencias internas se organizan en capas concéntricas, donde todas las flechas apuntan hacia el centro (**Capa de Dominio**).

```mermaid
graph TD
    subgraph Clean Architecture
        Infra[1. Capa de Infraestructura - Kafka Consumer, PostGIS, Express, WebSockets]
        Adapters[2. Capa de Adaptadores - Controllers, Event Mappers, Repositories]
        App[3. Capa de Aplicación - EvaluarRiesgoIncendio, EmitirAlertaEmergencia]
        Domain[4. Capa de Dominio - Entidades AnomaliaTermica, FocoDeCalor, Reglas >310K]

        Infra --> Adapters
        Adapters --> App
        App --> Domain
    end