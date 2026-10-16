# Arquitectura Orientada a Eventos (EDA) para Monitoreo de Puntos de Calor e Incendios Forestales en el Perú

**Curso:** Arquitectura de Software (IS-488)  
**Institución:** Universidad Nacional de San Cristóbal de Huamanga (UNSCH)  
**Escuela:** EP Ingeniería de Sistemas  
**Docente:** Mg. Ing. Richard Zapata Casaverde / Ing. Lizbeth Jaico Quispe  

---

## 👤 Datos del Estudiante
- **Nombre:** MAKS BRAYAN TANTA CHALA
- **Código:** 27221202
- **Repositorio GitHub:** [Incendios-EDA-arquitSoft-MAKS](https://github.com/Maks04092002/Incendios-EDA-arquitSoft-MAKS)

---

## 📌 Resumen del Proyecto
Este proyecto diseña una arquitectura de software orientada a eventos (EDA) y basada en microservicios para la ingesta, análisis y procesamiento en tiempo real de telemetría satelital (**NASA FIRMS, Copernicus, GOES-16**). El objetivo principal es reducir la latencia de detección de focos de calor e incendios forestales en el territorio peruano de horas a segundos, permitiendo la generación y distribución automática de alertas críticas a SERFOR, INDECI, COEN, autoridades regionales y brigadas de campo.

---

## 📁 Índice de Documentación del Repositorio

### 1. Análisis del Sistema
- [01 - Actores del Sistema](01-analisis-de-sistema/01-actores.md)
- [02 - Historias de Usuario](01-analisis-de-sistema/02-historias-del-usuario.md)
- [03 - Requisitos Funcionales](01-analisis-de-sistema/03-requisitos-funcionales.md)
- [04 - Atributos de Calidad](01-analisis-de-sistema/04-atributos-de-calidad.md)
- [05 - Restricciones del Sistema](01-analisis-de-sistema/05-restricciones.md)
- [06 - Drivers Arquitectónicos](01-analisis-de-sistema/06-driver-arquitectónicos.md)
- [07 - Decisiones Arquitectónicas](01-analisis-de-sistema/07_Desicion_Arquitectonica.md)
- [08 - Escenarios de Atributos de Calidad](01-analisis-de-sistema/08-escenarios-atributos-calidad.md)

### 2. Arquitectura de Software
- [Enfoque Arquitectónico](02-arquitectura-de-software/enfoque/enfoque-arquitectonico.md)
- [Arquitectura Inicial](02-arquitectura-de-software/arquitectura-inicial.md)
- [Estilo Arquitectónico](02-arquitectura-de-software/estilo-arquitectonico.md)
- [Componentes Arquitectónicos](02-arquitectura-de-software/componentes-arquitectonicos.md)

### 3. Diseño de Software (Diseño Interno)
- [Diseño Interno de Módulos (Consolidado)](03-diseño-de-software/diseño-interno/diseño-interno-de-modulos.md)
- [Principios de Diseño (SOLID)](03-diseño-de-software/diseño-interno/principios-de-diseño.md)
- [Patrones de Diseño (GoF y EDA)](03-diseño-de-software/diseño-interno/patrones-de-diseño.md)

### 4. Modelo C4
- [Nivel 1: Diagrama de Contexto del Sistema](04-modelo-c4/Nivel1-DiagramadeContextodelsistema.md)
- [Nivel 2: Diagrama de Contenedores](04-modelo-c4/Nivel2-DiagramadeContenedores.md)
- [Nivel 3: Diagrama de Componentes](04-modelo-c4/Nivel3-Diagrama-de-Componentes.md)
- [Nivel 4: Diagrama de Código](04-modelo-c4/Nivel4-DiagramadeCodigo.md)

---

## 🛠️ Tecnologías y Stack Técnico
- **Ingesta y Microservicios:** Python 3.11, FastAPI, Celery
- **Event Broker:** RabbitMQ / Apache Kafka (AMQP/TCP)
- **Motor CEP & Flujo:** Python CEP / RxPY / Apache Flink
- **Base de Datos Espacial:** PostgreSQL 16 + PostGIS 3.4 + TimescaleDB
- **Visualización & WebGIS:** React.js, Leaflet / Mapbox, WebSockets, Tailwind CSS
- **Fuentes Satelitales:** NASA FIRMS (VIIRS/MODIS), Copernicus (Sentinel), NOAA GOES-16

---

## 🏛️ Vista General de la Arquitectura

```mermaid
graph TB
    subgraph Fuentes ["📡 Fuentes Satelitales"]
        NASA["🛰️ NASA FIRMS API"]
        COP["🛰️ Copernicus"]
        GOES["🛰️ NOAA GOES-16"]
    end

    subgraph Ingesta ["⚡ Ingesta & Procesamiento"]
        INGEST["🐍 ms-ingesta-satelital"]
        BROKER[("🐇 RabbitMQ Broker")]
        CEP["🔥 ms-procesador-cep"]
    end

    subgraph Persistencia ["🗄️ Almacenamiento GIS"]
        DB[("PostgreSQL + PostGIS")]
    end

    subgraph Presentacion ["📢 Presentación & Alertas"]
        GATEWAY["🚀 ms-api-gateway"]
        NOTIF["🔔 ms-notificaciones-alertas"]
        WEBGIS["💻 Plataforma WebGIS"]
    end

    Fuentes --> INGEST
    INGEST -->|PuntoCalorDetectado| BROKER
    BROKER --> CEP
    CEP -->|AlertaGenerada| BROKER
    BROKER --> DB
    BROKER --> NOTIF
    GATEWAY --> DB
    NOTIF -.->|WebSockets / SMS| WEBGIS