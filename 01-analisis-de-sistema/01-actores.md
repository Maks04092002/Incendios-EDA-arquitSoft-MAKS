# Actores del Sistema

## Actores Humanos
| Actor | ¿Qué necesita realizar? |
| :--- | :--- |
| **Brigadista / Bombero Forestal** | Recibir alertas en tiempo real (SMS/Push) con la geolocalización exacta de focos de calor activos. |
| **Analista COEN / INDECI** | Monitorear el mapa de calor (WebGIS), evaluar niveles de riesgo y coordinar logística de respuesta. |
| **Gestor Ambiental SERFOR / MINAM** | Consultar datos históricos, evaluar afectación de áreas naturales protegidas y descargar reportes. |
| **Administrador del Sistema** | Monitorear la salud de los nodos de mensajería (Kafka), gestionar accesos (JWT) y configurar tópicos regionales. |

## Sistemas Externos y Fuentes de Datos
| Sistema Externo | Tipo de Integración | Función en el Sistema |
| :--- | :--- | :--- |
| **NASA FIRMS (VIIRS / MODIS)** | API REST / Polling | Ingesta continua de anomalías térmicas y puntos de calor activos. |
| **Copernicus (Sentinel-2/3)** | API REST / OData | Provisión de imágenes multiespectrales para cálculo de índices NDVI/NDWI. |
| **NOAA (GOES-16)** | Feeds en Tiempo Real | Datos satelitales geostacionarios de alta frecuencia temporal. |
| **SENAMHI** | API REST / Service | Datos meteorológicos locales (viento, humedad relativa, temperatura). |
| **Servicios de Notificación** | Webhook / SMTP / SMS Gateway | Difusión automatizada de alertas críticas a brigadas y autoridades. |