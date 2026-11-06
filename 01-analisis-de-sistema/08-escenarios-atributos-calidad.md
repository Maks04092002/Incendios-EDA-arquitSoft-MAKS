# Escenarios de Atributos de Calidad

Los siguientes escenarios son propuestas de validación. Las medidas no se
consideran resultados hasta ejecutar pruebas con datos y ambiente definidos.

| ID | Atributo | Estímulo | Respuesta esperada | Medición pendiente |
| :--- | :--- | :--- | :--- | :--- |
| ESC01 | Rendimiento | Llega una observación disponible desde la fuente | Ingesta, procesamiento y notificación registran marcas de tiempo separadas | Medir cada intervalo; no se fija una cifra sin evidencia |
| ESC02 | Escalabilidad | Aumenta el volumen de observaciones | Se agregan consumidores sin cambiar el productor | Definir carga, capacidad y pérdida aceptable |
| ESC03 | Resiliencia | La fuente o canal externo presenta timeout | Se aplica reintento o buffer según política y se registra el resultado | Medir recuperación, duplicados y eventos no entregados |
| ESC04 | Seguridad | Solicitud sin credenciales o con rol insuficiente | La API rechaza la operación y audita el intento | Medir respuestas y cobertura de permisos |
| ESC05 | Mantenibilidad | Se reemplaza un proveedor externo | Se modifica el adaptador sin cambiar reglas del dominio | Verificar pruebas de contrato y regresión |

Un punto de calor usado en estos escenarios sigue siendo una observación; no
se considera un incendio confirmado.
