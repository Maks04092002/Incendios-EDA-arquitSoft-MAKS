# Requisitos Funcionales

| ID | Descripción del Requisito Funcional |
| :--- | :--- |
| **RF01** | El sistema debe consumir continuamente eventos térmicos desde las APIs de NASA FIRMS, Copernicus y GOES-16. |
| **RF02** | El sistema debe publicar los datos satelitales estandarizados (GeoJSON) en un Broker de Mensajes distribuido. |
| **RF03** | El sistema debe procesar en flujo (CEP) la temperatura de brillo (> 310 K) y calcular dinámicamente el nivel de riesgo (Bajo, Medio, Alto, Crítico). |
| **RF04** | El sistema debe cruzar la posición del foco de calor con mapas de vegetación y factores meteorológicos (SENAMHI). |
| **RF05** | El sistema debe emitir notificaciones automáticas multicanal (SMS, Email, Webhooks) cuando el riesgo alcance nivel Alto o Crítico. |
| **RF06** | El sistema debe renderizar capas GIS, mapas de calor (Heatmaps) y marcadores en vivo sobre una plataforma WebGIS. |
| **RF07** | El sistema debe almacenar eventos geográficos e históricamente en una base de datos espacial (PostgreSQL + PostGIS + TimescaleDB). |
| **RF08** | El sistema debe restringir el acceso mediante autenticación basada en tokens JWT y roles de usuario. |

## Mapeo entre Historias de Usuario y Requisitos Funcionales

| Historia de Usuario | Requisitos Funcionales Relacionados |
| :--- | :--- |
| **HU01**: Visualización WebGIS | RF01, RF06 |
| **HU02**: Alertas instantáneas | RF03, RF05 |
| **HU03**: Correlación con capas ANP | RF03, RF04 |
| **HU04**: Bus de eventos regional | RF02 |
| **HU05**: Exportación de reportes | RF06, RF07 |
| **HU06**: Persistencia histórica | RF07, RF08 |