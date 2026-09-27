# Arquitectura Orientada a Eventos (EDA) para Detección de Incendios Forestales en el Perú

**Curso:** Arquitectura de Software (IS-488)  
**Institución:** Universidad Nacional de San Cristóbal de Huamanga (UNSCH)  
**Escuela** EP Ingenieria de Sistemas
**Docente:** Mg. Ing. Richard Zapata Casaverde  / Ing. Lizbeth Jaico  Quispe

---

## 👤 Datos del Estudiante
* **Nombre:** MAKS BRAYAN TANTA CHALA
* **Código:** 27221202
* **Repositorio GitHub:** [Incendios-EDA-arquitSoft-MAKS](https://github.com/Maks04092002/Incendios-EDA-arquitSoft-MAKS)

---

## 📌 Resumen del Proyecto
Este proyecto diseña una arquitectura de software orientada a eventos (EDA) para la ingesta, análisis y procesamiento en tiempo real de telemetría satelital (NASA FIRMS, Copernicus, GOES-16). El objetivo es reducir la latencia de detección de focos de calor a nivel nacional de horas a segundos, distribuyendo alertas críticas a SERFOR, INDECI, COEN y brigadas de campo.

---

## 📁 Índice de Documentación

### 1. Análisis del Sistema
* [01 - Actores del Sistema](analisis-de-sistema/01-actores.md)
* [02 - Historias de Usuario](analisis-de-sistema/02-historias-del-usuario.md)
* [03 - Requisitos Funcionales](analisis-de-sistema/03-requisitos-funcionales.md)
* [04 - Atributos de Calidad](analisis-de-sistema/04-atributos-de-calidad.md)
* [05 - Restricciones del Sistema](analisis-de-sistema/05-restricciones.md)
* [06 - Drivers Arquitectónicos](analisis-de-sistema/06-driver-arquitectónicos.md)

### 2. Diseño de Arquitectura
* [Arquitectura Técnica y Distribución de Servidores](arquitectura/arquitectura-inicial.md)

---

## 🛠️ Tecnologías y Arquitectura
* **Event Broker:** Apache Kafka / RabbitMQ
* **Stream Processing Engine:** Apache Flink / Spark Streaming (CEP)
* **Persistencia Espacial:** PostgreSQL + PostGIS + TimescaleDB
* **Capa de Presentación:** WebGIS (React + Mapbox), API Gateway (Nginx/FastAPI), WebSockets
* **Fuentes Satelitales:** NASA FIRMS (VIIRS/MODIS), Copernicus, NOAA GOES-16
