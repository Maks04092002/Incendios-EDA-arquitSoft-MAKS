# Arquitectura Orientada a Eventos (EDA) para Monitoreo de Puntos de Calor e Incendios Forestales en el Perú

**Curso:** Arquitectura de Software (IS-488)  
**Institución:** Universidad Nacional de San Cristóbal de Huamanga (UNSCH)

## Objetivo

Diseñar una arquitectura orientada a eventos para recibir observaciones de
puntos de calor satelitales, evaluarlas experimentalmente y apoyar el
monitoreo geoespacial y la distribución de alertas. Una observación de punto
de calor no se interpreta como confirmación de incendio.

## Alcance del Sprint 1

- Definición de actores, historias y requisitos.
- Separación de seis módulos funcionales.
- Diseño propuesto EDA, Clean Architecture y arquitectura hexagonal.
- Vistas C4 editables en Mermaid.
- Boilerplate Angular con pruebas de dominio independiente, heredado como
  material de referencia para separación de capas.

La integración satelital, los servicios Python/FastAPI, RabbitMQ,
PostgreSQL/PostGIS y el WebGIS de monitoreo todavía no están implementados en
este repositorio.

## Estilo arquitectónico y módulos funcionales

La **Arquitectura Orientada a Eventos (EDA)** es el estilo principal. La
comunicación entre productores y consumidores usa eventos asíncronos mediante
RabbitMQ, mientras que las consultas del WebGIS y la administración usan una
API REST síncrona. Microservicios, arquitectura en capas, Clean Architecture y
arquitectura hexagonal son enfoques complementarios; SOLID y los patrones son
principios de diseño interno, no estilos arquitectónicos.

La vista modular y el flujo de eventos se encuentran en
[«Vista modular y flujo de eventos – EDA»](02-arquitectura-de-software/estilo-arquitectonico.md).

1. **Gestión de Datos Satelitales:** incorporación, adaptación y validación de observaciones.
2. **Gestión de Puntos de Calor:** registro, consulta y conservación de observaciones.
3. **Evaluación Experimental del Riesgo:** aplicación de reglas versionadas y pendientes de validación.
4. **Monitoreo Geoespacial:** consulta y visualización territorial.
5. **Gestión de Alertas:** generación y distribución de notificaciones configuradas.
6. **Administración del Sistema:** usuarios, permisos, parámetros y auditoría.

RabbitMQ, PostgreSQL, PostGIS y FastAPI son infraestructura o tecnologías,
no módulos funcionales. Los microservicios son una posible división de
implementación.

## Estilo y tecnologías previstas

EDA es el estilo principal. Clean Architecture y arquitectura hexagonal
separan dominio, aplicación, puertos y adaptadores. Los patrones Adapter,
Strategy, Repository y Publish/Subscribe son decisiones de diseño propuestas.

| Área | Tecnología prevista | Estado |
| :--- | :--- | :--- |
| Fuente | NASA FIRMS | No integrada |
| Backend | Python + FastAPI | No presente |
| Eventos | RabbitMQ | No presente |
| Datos | PostgreSQL + PostGIS | No presente |
| WebGIS | React + Leaflet | No presente; el boilerplate usa Angular |

## Documentación

### Análisis del sistema

- [Actores](01-analisis-de-sistema/01-actores.md)
- [Historias de usuario](01-analisis-de-sistema/02-historias-del-usuario.md)
- [Requisitos funcionales](01-analisis-de-sistema/03-requisitos-funcionales.md)
- [Atributos de calidad](01-analisis-de-sistema/04-atributos-de-calidad.md)
- [Medición de atributos de calidad](01-analisis-de-sistema/medicion-atributos-calidad.md)
- [Restricciones](01-analisis-de-sistema/05-restricciones.md)
- [Drivers arquitectónicos](01-analisis-de-sistema/06-driver-arquitectónicos.md)
- [Decisiones arquitectónicas](01-analisis-de-sistema/07_Desicion_Arquitectonica.md)
- [Escenarios de calidad](01-analisis-de-sistema/08-escenarios-atributos-calidad.md)

### Arquitectura y diseño

- [Arquitectura inicial en capas](02-arquitectura-de-software/arquitectura-inicial.md)
- [Estilo arquitectónico](02-arquitectura-de-software/estilo-arquitectonico.md)
- [Componentes y módulos](02-arquitectura-de-software/componentes-arquitectonicos.md)
- [Diseño interno](03-diseño-de-software/diseño-interno/diseño-interno-de-modulos.md)
- [Principios SOLID](03-diseño-de-software/diseño-interno/principios-de-diseño.md)
- [Patrones](03-diseño-de-software/diseño-interno/patrones-de-diseño.md)

### Modelo C4

- [C1 — Contexto](04-modelo-c4/Nivel1-DiagramadeContextodelSistema.md)
- [C2 — Contenedores](04-modelo-c4/Nivel2-DiagramadeContenedores.md)
- [C3 — Componentes](04-modelo-c4/Nivel3-Diagrama-de-Componentes.md)
- [C4 — Código](04-modelo-c4/Nivel4-DiagramadeCodigo.md)
- [Vista dinámica — Secuencia](04-modelo-c4/Vista-Dinamica-Secuencia.md)

## Código y ejecución comprobada

El código real está en [boilerplate-main/boilerplate-main](boilerplate-main/boilerplate-main)
y corresponde a un marketplace Angular de productos para mascotas. Sus
comandos se documentan en su [README](boilerplate-main/boilerplate-main/README.md).
No se declara aquí que esos comandos ejecuten el sistema satelital.

## Próximos pasos

1. Validar las reglas experimentales con datos y criterios del dominio.
2. Definir contratos de eventos, versionado, idempotencia y reintentos.
3. Implementar de forma incremental ingesta, procesamiento, persistencia,
   consulta y alertas.
4. Incorporar pruebas de contrato, integración y mediciones separadas de
   observación, procesamiento y notificación.

Para el detalle de la auditoría y de los pendientes, consulte
[REVISION_ARQUITECTURA.md](REVISION_ARQUITECTURA.md).
