# Revisión de Arquitectura

## Alcance de la revisión

Se revisaron el README, los documentos de análisis, arquitectura, diseño,
modelo C4 y el código presente en `boilerplate-main/boilerplate-main`.
No se realizaron commits, push, merge ni rebase.

## Hallazgos principales

1. La documentación mezclaba módulos funcionales, microservicios,
   infraestructura y tecnologías.
2. Se presentaban Python/FastAPI, RabbitMQ, PostgreSQL/PostGIS y React como
   implementación, aunque no aparecen en el código inspeccionado.
3. Existían alternativas tecnológicas simultáneas sin una decisión
   consistente: Kafka/RabbitMQ, React/Angular, Flink/Spark/RxPY.
4. El C2 conectaba procesamiento o broker con la base de datos sin mostrar
   explícitamente un consumidor de persistencia.
5. El documento C4 contenía una secuencia y no una vista de código.
6. Las reglas térmicas y cifras de latencia se expresaban como si fueran
   criterios validados.
7. Las historias y requisitos no tenían una trazabilidad completa y HU06
   estaba relacionada incorrectamente con autenticación.
8. Los patrones se describían como aplicados aunque las clases del sistema
   aún no existen.

## Cambios realizados

| Archivo | Cambio |
| :--- | :--- |
| `01-analisis-de-sistema/02-historias-del-usuario.md` | Historias alineadas con observaciones, evaluaciones experimentales y estado futuro. |
| `01-analisis-de-sistema/03-requisitos-funcionales.md` | Requisitos reorganizados, trazabilidad HU-RF y reglas de interpretación. |
| `02-arquitectura-de-software/componentes-arquitectonicos.md` | Seis módulos, servicios propuestos y flujo EDA con consumidor de persistencia. |
| `02-arquitectura-de-software/estilo-arquitectonico.md` | EDA, capas, Clean/Hexagonal, tecnologías previstas y patrones diferenciados. |
| `04-modelo-c4/Nivel1-DiagramadeContextodelSistema.md` | C1 limitado a personas, sistema y sistemas externos. |
| `04-modelo-c4/Nivel2-DiagramadeContenedores.md` | C2 con WebGIS, servicios, RabbitMQ, PostgreSQL/PostGIS y persistencia explícita. |
| `04-modelo-c4/Nivel3-Diagrama-de-Componentes.md` | C3 del servicio de procesamiento y sus puertos/adaptadores. |
| `04-modelo-c4/Nivel4-DiagramadeCodigo.md` | C4 de código propuesto para un componente concreto. |
| `04-modelo-c4/Vista-Dinamica-Secuencia.md` | Secuencia separada de las vistas C4. |
| `README.md` | Objetivo, alcance, estado real, enlaces y próximos pasos. |

## Estado real del código

El único proyecto con manifest y código funcional comprobable es Angular 18
en `boilerplate-main/boilerplate-main`. Implementa un marketplace con dominio,
aplicación, infraestructura y presentación, además de pruebas de dominio.
Los directorios con nombres de servicios satelitales están vacíos. No hay
evidencia local de una integración NASA FIRMS, RabbitMQ, PostgreSQL/PostGIS,
FastAPI, React o Leaflet.

## Matriz de trazabilidad

| Historia | Requisito | Módulo | Componente propuesto | Evidencia o prueba |
| :--- | :--- | :--- | :--- | :--- |
| HU01 | RF06, RF07 | Monitoreo Geoespacial | API de Consulta + WebGIS | Pendiente: prueba de consulta y renderizado |
| HU02 | RF04, RF08 | Gestión de Alertas | Servicio de Procesamiento + Servicio de Alertas | Pendiente: prueba de regla y entrega |
| HU03 | RF05, RF07 | Monitoreo Geoespacial | Servicio de Procesamiento + API de Consulta | Pendiente: prueba de correlación geográfica |
| HU04 | RF09 | Administración del Sistema | Servicio de Administración | Pendiente: prueba de roles y configuración |
| HU05 | RF06, RF10 | Gestión de Puntos de Calor | API de Consulta y Exportación | Pendiente: prueba de consulta y exportación |
| HU06 | RF06 | Gestión de Puntos de Calor | Servicio de Persistencia Geoespacial | Pendiente: prueba de persistencia histórica |

Las evidencias marcadas como pendientes no son pruebas exitosas. Las pruebas
existentes del boilerplate validan reglas del marketplace, no estos requisitos.

## Pendientes

- Definir contratos y esquema versionado de eventos.
- Seleccionar y configurar definitivamente el broker.
- Implementar los servicios propuestos sin sustituir el código funcional
  existente sin justificación.
- Validar reglas de riesgo con datos representativos y expertos del dominio.
- Definir autenticación, autorización, auditoría e idempotencia.
- Crear pruebas unitarias, de contrato, integración y rendimiento.
- Medir por separado disponibilidad satelital, procesamiento y notificación.
