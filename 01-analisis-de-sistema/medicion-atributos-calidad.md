# Medición de Atributos de Calidad

## 1. Propósito y estado

Este documento convierte los atributos de calidad del proyecto en indicadores
observables, procedimientos reproducibles y evidencias auditables. Describe
pruebas futuras para el sistema EDA propuesto; no contiene resultados
inventados.

**Estado global:** NO EJECUTADO. El código actual es un boilerplate Angular de
marketplace; todavía no existen los servicios Python/FastAPI, RabbitMQ,
PostgreSQL/PostGIS, NASA FIRMS ni el WebGIS de monitoreo.

Las cargas se deben ejecutar con datos locales o respuestas simuladas. No se
deben realizar cargas intensivas contra NASA FIRMS.

## 2. Relación con requisitos no funcionales

| Requisito | Atributos y escenarios relacionados | Evidencia |
| :--- | :--- | :--- |
| AC01 Rendimiento | AC01, EC-REN-01 | Trazas, percentiles y throughput |
| AC02 Escalabilidad | AC02, EC-ESC-01 | Comparación de consumidores, colas y recursos |
| AC03 Disponibilidad y resiliencia | AC03, AC04, EC-FIA-01, EC-DIS-01 | Logs de fallos, recuperación y estado |
| AC04 Seguridad | AC05, EC-SEG-01 | Resultados de autorización y análisis |
| AC05 Mantenibilidad y desacoplamiento | AC06, EC-MAN-01 | Cobertura, complejidad y dependencias |
| RF01–RF10 | AC07, EC-INT-01 y escenarios funcionales | Contratos, registros y pruebas de integración |
| HU01–HU06 | AC08, EC-USA-01 y pruebas funcionales | Registro de tareas y cuestionario |

Los nombres AC01–AC05 conservan los atributos ya definidos. AC06–AC08
detallan mantenibilidad, interoperabilidad y usabilidad para hacerlos
medibles.

## 3. Indicadores y fórmulas

### 3.1 Rendimiento

- **Latencia de procesamiento:** `t_confirmacion - t_recepcion`. Inicia cuando
  el consumidor recibe el mensaje y termina cuando finaliza el procesamiento y
  confirma el mensaje. No incluye el tiempo de disponibilidad satelital.
- **Latencia extremo a extremo interna:** `t_notificacion_solicitada -
  t_publicacion`. Debe separar publicación, consumo, persistencia y solicitud
  de notificación.
- **Latencia REST:** `t_respuesta - t_solicitud`, medida en el cliente o
  servidor con una definición única.
- **Throughput ofrecido:** `eventos_ingresados / intervalo`.
- **Throughput procesado:** `eventos_procesados / intervalo`.
- **Throughput confirmado:** `eventos_confirmados / intervalo`.
- **Tasa de sobreobjetivo:** `solicitudes_fuera_objetivo / solicitudes_totales
  × 100`.
- Reportar p50, p95 y p99 sobre las observaciones ordenadas. Para una muestra
  de tamaño `n`, usar el percentil elegido con un método documentado por la
  herramienta; conservar la muestra completa para reproducibilidad.

Los objetivos de milisegundos, throughput y porcentaje quedan **pendientes de
establecer mediante línea base**.

### 3.2 Escalabilidad

- `eventos/segundo`, longitud máxima y promedio de cola, tiempo de espera del
  mensaje, CPU y memoria por consumidor.
- **Incremento de rendimiento:** `(throughput_N - throughput_1) /
  throughput_1 × 100`.
- Comparar la misma carga y ventana con un consumidor y con varios. Registrar
  número de instancias, concurrencia, recursos y configuración del broker.

### 3.3 Fiabilidad

- **Procesamiento exitoso:** `eventos_confirmados_sin_error /
  eventos_recibidos × 100`.
- **Tasa de error:** `eventos_fallidos / eventos_recibidos × 100`.
- **Duplicados:** contar claves de evento repetidas después de aplicar la
  política de idempotencia.
- **Recuperación:** `eventos_recuperados / eventos_fallidos_reintentables × 100`.
- Distinguir publicado, entregado, procesado y confirmado mediante identificador
  de evento, timestamps y logs correlacionados.

### 3.4 Disponibilidad y recuperación

- **Disponibilidad:** `(tiempo_operativo / tiempo_total_observado) × 100`.
- **MTTR:** `tiempo_total_de_recuperacion / numero_de_incidentes`.
- Medir desde el primer fallo observable hasta la restauración del servicio.
  RTO y RPO permanecen pendientes hasta ser definidos y justificados.

### 3.5 Seguridad

- **Autorización correcta:** `casos_autorizados_correctamente /
  casos_de_prueba × 100`.
- Número de rutas sensibles sin protección.
- Número de secretos detectados por herramienta.
- Número y severidad de vulnerabilidades reportadas.
- Probar con credenciales ficticias y variables de entorno; nunca incluir
  secretos reales.

### 3.6 Mantenibilidad

- Cobertura: `líneas o ramas cubiertas / líneas o ramas medibles × 100`.
- Complejidad ciclomática por función y módulo.
- Número de dependencias entre capas y violaciones de reglas arquitectónicas.
- Número de pruebas afectadas por un cambio controlado.

Son indicadores parciales: una cobertura alta o baja complejidad no demuestra
por sí sola buena mantenibilidad.

### 3.7 Interoperabilidad

- **Registros válidos:** `registros_aceptados / registros_recibidos × 100`.
- **Mensajes conformes:** `mensajes_validos_esquema /
  mensajes_evaluados × 100`.
- Número de errores de integración y porcentaje de pruebas de contrato
  satisfactorias.
- Probar formatos CSV/JSON y diferencias documentadas de productos
  satelitales; no asumir que confianza o temperatura tienen el mismo
  significado en todas las fuentes.

### 3.8 Usabilidad del WebGIS

- **Tareas completadas:** `tareas_completadas / tareas_asignadas × 100`.
- Tiempo por tarea, errores de interacción y valoración mediante un
  cuestionario previamente definido.
- Las pruebas deben usar usuarios de prueba y datos controlados; no se
  inventan resultados.

## 4. Matriz de métricas

| ID | Atributo | Métrica | Método | Unidad | Objetivo | Herramienta | Evidencia | Estado |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| M-REN-01 | Rendimiento | p50/p95/p99 de procesamiento | Timestamps recepción-confirmación | ms | Pendiente de línea base | Logs, Python | CSV y configuración | NO EJECUTADO |
| M-REN-02 | Rendimiento | Throughput confirmado | Confirmados/intervalo | eventos/s | Pendiente de línea base | Python | CSV y gráfico | NO EJECUTADO |
| M-REN-03 | Rendimiento | Latencia REST | Solicitud-respuesta | ms | Pendiente de línea base | pytest, Locust o k6 | Reporte HTTP | NO EJECUTADO |
| M-ESC-01 | Escalabilidad | Throughput por consumidores | Comparación N/1 | eventos/s | Pendiente de línea base | RabbitMQ Management | Exportación y recursos | NO EJECUTADO |
| M-ESC-02 | Escalabilidad | Longitud de cola | Promedio y máximo | mensajes | Sin crecimiento sostenido | RabbitMQ Management | Métricas de cola | NO EJECUTADO |
| M-FIA-01 | Fiabilidad | Procesamiento exitoso | Confirmados sin error/recibidos | % | Pendiente de línea base | Logs, pytest | IDs y resultados | NO EJECUTADO |
| M-FIA-02 | Fiabilidad | Duplicados | Claves repetidas | cantidad | Pendiente de línea base | SQL/Python | Informe de integridad | NO EJECUTADO |
| M-DIS-01 | Disponibilidad | Disponibilidad | Operativo/observado | % | Pendiente de definir | Monitor y logs | Línea temporal | NO EJECUTADO |
| M-DIS-02 | Disponibilidad | MTTR | Recuperación/incidentes | min | Pendiente de definir | Logs | Registro de incidentes | NO EJECUTADO |
| M-SEG-01 | Seguridad | Autorizaciones correctas | Casos correctos/casos | % | 100% en casos definidos | pytest y análisis | Reporte de seguridad | NO EJECUTADO |
| M-SEG-02 | Seguridad | Rutas sensibles sin protección | Inspección y pruebas | cantidad | 0 en alcance definido | pytest, Ruff u otra herramienta | Lista de rutas | NO EJECUTADO |
| M-MAN-01 | Mantenibilidad | Cobertura | Cubierto/medible | % | Pendiente de línea base | coverage.py o pytest-cov | Reporte de cobertura | NO EJECUTADO |
| M-MAN-02 | Mantenibilidad | Complejidad | Complejidad por función | índice | Pendiente de línea base | Herramienta estática | Reporte | NO EJECUTADO |
| M-INT-01 | Interoperabilidad | Registros válidos | Aceptados/recibidos | % | Pendiente de línea base | pytest | Fixtures y resultados | NO EJECUTADO |
| M-INT-02 | Interoperabilidad | Contratos conformes | Válidos/evaluados | % | 100% de casos definidos | Validador de esquema | Reporte de contrato | NO EJECUTADO |
| M-USA-01 | Usabilidad | Tareas completadas | Completadas/asignadas | % | Pendiente de instrumento | Prueba moderada | Formulario y tiempos | NO EJECUTADO |
| M-USA-02 | Usabilidad | Tiempo por tarea | Inicio-fin | s o min | Pendiente de línea base | Cronómetro/formulario | Registro anonimizado | NO EJECUTADO |

## 5. Plan reproducible de pruebas

### PR-01 — Procesamiento y latencia

**Objetivo:** medir procesamiento de eventos con datos simulados.  
**Requisitos previos:** servicios implementados, RabbitMQ local, consumidor
configurado y reloj coherente.  
**Datos:** 1000 eventos sintéticos válidos y un conjunto inválido separado.  
**Configuración:** registrar CPU, memoria, versiones, instancias y
consumidores.  
**Procedimiento:** publicar eventos, registrar timestamps de publicación,
recepción, procesamiento y confirmación; repetir con una configuración
documentada.  
**Métricas:** p50/p95/p99, throughput ofrecido/procesado/confirmado, errores y
cola.  
**Criterio:** comparar contra la línea base aprobada; no declarar aprobación
antes de definirla.  
**Evidencia:** configuración, fixtures, logs, CSV y script de análisis.

### PR-02 — Escalamiento de consumidores

**Objetivo:** comparar uno y varios consumidores.  
**Procedimiento:** repetir la misma carga con 1, 2 y N consumidores, sin
cambiar los demás parámetros; observar throughput, cola, CPU y memoria.  
**Criterio:** documentar el incremento y cualquier saturación; no asumir
escalamiento lineal.  
**Evidencia:** configuración RabbitMQ, métricas exportadas y tabla comparativa.

### PR-03 — Fallos, duplicados e idempotencia

**Objetivo:** verificar confirmaciones, reintentos y consistencia.  
**Procedimiento:** detener/reiniciar un consumidor, simular indisponibilidad de
la base de datos, reenviar un evento y revisar la clave de idempotencia.  
**Criterio:** no confirmar antes del efecto requerido y no duplicar persistencia
cuando el contrato lo prohíba.  
**Evidencia:** secuencia temporal de logs, IDs, estados del repositorio y
conteo de recuperados.

### PR-04 — Disponibilidad y recuperación

**Objetivo:** medir recuperación de API y consumidores.  
**Procedimiento:** ejecutar una interrupción controlada en entorno aislado,
registrar inicio del fallo, detección y restauración.  
**Criterio:** comparar MTTR y disponibilidad con objetivos aprobados.  
**Evidencia:** incidentes, logs, timestamps y configuración del entorno.

### PR-05 — Seguridad e interoperabilidad

**Objetivo:** verificar roles, entradas y contratos.  
**Procedimiento:** ejecutar casos permitidos y denegados con credenciales
ficticias; validar fixtures CSV/JSON y versiones de eventos.  
**Criterio:** todas las rutas sensibles del alcance deben tener controles y los
casos de contrato definidos deben conformar.  
**Evidencia:** reporte de pruebas, esquemas, fixtures y análisis estático.

### PR-06 — Mantenibilidad y usabilidad

**Objetivo:** medir indicadores parciales de código y tareas del WebGIS.  
**Procedimiento:** ejecutar cobertura/análisis estático y, cuando exista el
WebGIS, observar tareas de consulta, filtro y lectura de alertas con usuarios
de prueba.  
**Criterio:** comparar con línea base y registrar desviaciones.  
**Evidencia:** reportes de cobertura/complejidad, cuestionario y tiempos
anonimizados.

## 6. Plantilla de resultado

```text
ID de prueba:
Fecha de ejecución:
Commit o versión:
Entorno y recursos:
Datos utilizados:
Configuración (instancias/consumidores):
Métricas recogidas:
Resultado obtenido:
Objetivo:
Cumplimiento: NO EJECUTADO
Observaciones:
Archivo o captura de evidencia:
```

## 7. Relación con decisiones EDA

- RabbitMQ puede desacoplar temporalmente productores y consumidores, pero
  introduce consistencia eventual, duplicados, reintentos y operación del
  broker.
- Consumidores concurrentes pueden aumentar la capacidad observada, pero deben
  medirse colas, orden requerido, CPU, memoria y comportamiento de mensajes
  duplicados.
- REST simplifica consultas síncronas, pero su latencia depende de API,
  repositorio y consulta geoespacial.
- PostGIS puede resolver consultas espaciales del dominio, pero su rendimiento
  depende de modelo, índices, volumen y consulta; debe verificarse con
  `EXPLAIN ANALYZE` cuando exista la base.
- Clean Architecture y puertos/adaptadores reducen acoplamiento previsto y
  facilitan pruebas, pero no garantizan mantenibilidad sin diseño y pruebas
  disciplinadas.
- Confirmaciones e idempotencia mejoran la fiabilidad prevista, a cambio de
  complejidad de estado, claves y recuperación.

## 8. Pendientes

1. Implementar el primer flujo vertical con eventos locales.
2. Definir esquemas y claves de idempotencia.
3. Seleccionar hardware/entorno y aprobar una línea base.
4. Definir objetivos RTO/RPO si el alcance los requiere.
5. Implementar pruebas unitarias, de contrato, integración, carga y usabilidad.
6. Ejecutar las pruebas y completar las plantillas sin convertir objetivos en
   resultados.
