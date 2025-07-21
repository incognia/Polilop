# ROADMAP - Polilop v2.0
## Sistema integral de seguimiento postal

### Visión general del desarrollo

El desarrollo del proyecto Polilop v2.0 se estructura en **4 fases principales** que abarcan desde la fase de investigación hasta el despliegue piloto. El proyecto incluye tres componentes principales: dispositivo escáner autónomo, aplicación móvil Android, y dashboard web administrativo.

**Fecha de inicio**: 1 de agosto de 2024
**Duración estimada**: 24 semanas (6 meses)
**Fecha estimada de finalización**: 31 de enero de 2025

---

## Fase 1: Investigación y especificaciones (3 semanas)
**Período**: 1 de agosto - 22 de agosto de 2024

### 🔬 Investigación técnica (Paralelo)
- **Semana 1**: Evaluación intensiva SQLite vs JSON
  - Implementación y pruebas de consumo simultáneas
  - Decisión técnica inmediata basada en métricas
  
- **Semana 2**: Validación de componentes + Documentación API
  - Pruebas de compatibilidad SIM7600G-H con ESP32
  - Validación de autonomía con batería Li-Po 4000mAh
  - Especificación simultánea de API REST

- **Semana 3**: Finalización de especificaciones
  - Protocolos de comunicación BLE definidos
  - Esquemas de base de datos PostgreSQL/PostGIS
  - Documentación técnica completada

### 🎯 Entregables Fase 1
- [ ] Decisión técnica: SQLite vs JSON para almacenamiento local
- [ ] Validación completa de componentes hardware
- [ ] Especificaciones técnicas finalizadas
- [ ] Documentación de API REST completa
- [ ] Protocolo BLE para configuración definido

---

## Fase 2: Desarrollo de firmware y hardware (7 semanas)
**Período**: 22 de agosto - 10 de octubre de 2024

### 🔧 Desarrollo de hardware (5 semanas - Intensivo)
- **Semana 1-2**: Diseño acelerado de esquemáticos y PCB
  - PCB principal con ESP32-WROOM-32U (diseño simultáneo)
  - Circuitos de gestión de energía y protección
  - Módulos intercambiables (batería y escáner)
  - Enrutado optimizado en paralelo

- **Semana 3**: Validación y optimización de diseño
  - Optimización de señales RF (WiFi/Bluetooth/Celular)
  - Validación de restricciones físicas y térmicas
  - Finalización de diseños para fabricación

- **Semana 4-5**: Fabricación rápida de prototipos
  - Pedido express de PCBs y componentes
  - Ensamblaje de primeros prototipos
  - Pruebas básicas de funcionalidad

### 💻 Desarrollo de firmware (7 semanas - Paralelo desde semana 1)
- **Semana 1-2**: Configuración base y drivers (simultáneo con hardware)
  - Configuración de MicroPython en ESP32
  - Drivers para módulos periféricos (GPS, escáner, celular)
  - Sistema base de gestión de energía

- **Semana 3-4**: Funcionalidades core
  - Implementación de escáner de códigos
  - Sistema de almacenamiento local (SQLite/JSON)
  - Georreferenciación GPS con timestamps UTC

- **Semana 5-6**: Conectividad y sincronización
  - Conectividad WiFi y celular con fallback inteligente
  - Protocolo de sincronización con API REST
  - Sistema de colas y reintentos

- **Semana 7**: Configuración BLE y refinamiento
  - Protocolo Bluetooth LE para configuración
  - Sistema de LEDs y buzzer para feedback
  - Optimización de consumo energético

### 🎯 Entregables Fase 2
- [ ] Esquemáticos completos en KiCad
- [ ] PCBs diseñadas y fabricadas (primera iteración)
- [ ] 3-5 prototipos funcionales ensamblados
- [ ] Firmware base con funcionalidades core
- [ ] Sistema de almacenamiento local implementado
- [ ] Protocolo BLE de configuración funcional

---

## Fase 3: Desarrollo de aplicaciones (8 semanas)
**Período**: 10 de octubre - 5 de diciembre de 2024

### 📱 Aplicación móvil Android (5 semanas - Intensivo)
- **Semana 1-2**: Base y BLE con desarrollo paralelo UI
  - Configuración del proyecto Android (API 26+) + Diseño UI
  - Implementación de BLE + Pantallas de conexión simultáneas
  - Emparejamiento y bases de configuración en paralelo

- **Semana 3**: Configuración de dispositivos (intensivo)
  - Interfaz WiFi y celular desarrolladas simultáneamente
  - Dashboard del estado del dispositivo integrado
  - Validación cruzada de funcionalidades

- **Semana 4-5**: OTA, diagnósticos e integración
  - Sistema de actualización OTA de firmware
  - Módulo de diagnósticos del dispositivo
  - Testing integral y refinamiento UI

### 🌐 Dashboard web administrativo (8 semanas - Paralelo total)
- **Semana 1-2**: API REST + Base de datos + Arquitectura frontend
  - PostgreSQL con PostGIS configurado
  - API REST con JWT + Esquemas de BD simultáneos
  - Arquitectura frontend y setup del proyecto web

- **Semana 3-4**: Backend procesamiento + Frontend core
  - Servicios de procesamiento de escaneos
  - Dashboard principal con métricas desarrollado en paralelo
  - Sistema de autenticación web + Gestión de dispositivos

- **Semana 5-6**: Notificaciones + Mapas interactivos
  - Sistema de notificaciones en tiempo real
  - Mapas interactivos con visualización georreferenciada
  - Mapas de calor de entregas y tiempos

- **Semana 7-8**: Reportes + Integración completa
  - Generador de reportes avanzados
  - Integración completa con dispositivos y app móvil
  - Optimización de rendimiento, cache (Redis) y escalabilidad

### 🎯 Entregables Fase 3
- [ ] Aplicación Android funcional con todas las características
- [ ] API REST completa con autenticación
- [ ] Base de datos PostgreSQL/PostGIS configurada
- [ ] Dashboard web con mapas interactivos
- [ ] Sistema de reportes y análisis implementado
- [ ] Integración completa dispositivo-app-web

---

## Fase 4: Testing, optimización y piloto (6 semanas)
**Período**: 5 de diciembre de 2024 - 16 de enero de 2025

### 🧪 Testing integral (3 semanas)
- **Semana 1**: Testing de hardware
  - Pruebas de resistencia y durabilidad
  - Validación de autonomía en condiciones reales
  - Pruebas de conectividad en diferentes zonas

- **Semana 2**: Testing de software
  - Pruebas de integración extremo a extremo
  - Testing de carga del sistema web
  - Validación de sincronización en escenarios complejos

- **Semana 3**: Testing de usuario
  - Pruebas de usabilidad con carteros reales
  - Validación de procesos operativos
  - Ajustes basados en feedback

### 🚀 Optimización y despliegue (3 semanas)
- **Semana 4**: Optimización final
  - Optimización de consumo energético
  - Mejoras de rendimiento del sistema web
  - Refinamiento de interfaces de usuario

- **Semana 5-6**: Preparación del piloto
  - Configuración de infraestructura de producción
  - Capacitación de usuarios piloto
  - Documentación de usuario final
  - Procedimientos de mantenimiento

### 🎯 Entregables Fase 4
- [ ] Sistema completamente probado e integrado
- [ ] Documentación de usuario completa
- [ ] Infraestructura de producción configurada
- [ ] Programa piloto implementado
- [ ] Procedimientos de mantenimiento establecidos

---

## Recursos y dependencias

### 👥 Equipo requerido
- **Hardware Developer**: Diseño de PCB y validación de prototipos
- **Firmware Developer**: Desarrollo en MicroPython/ESP32
- **Android Developer**: Aplicación móvil nativa
- **Full-stack Developer**: API REST y dashboard web
- **DevOps Engineer**: Infraestructura y despliegue
- **QA Tester**: Testing integral y validación

### 🔧 Herramientas y tecnologías
- **Hardware**: KiCad, multímetros de precisión, estaciones de soldadura
- **Firmware**: MicroPython, ESP32-IDF, herramientas de debug
- **Mobile**: Android Studio, herramientas BLE testing
- **Backend**: PostgreSQL/PostGIS, Redis, servidores Linux
- **Frontend**: Framework web moderno, librerías de mapas
- **Infraestructura**: Contenedores, Kubernetes/Argo

### ⚠️ Riesgos y mitigaciones
1. **Retrasos en fabricación de PCBs**: Buffer de 2 semanas incluido
2. **Problemas de consumo energético**: Fase de investigación extendida
3. **Complejidad de integración BLE**: Prototipado temprano
4. **Disponibilidad de componentes**: Identificación de alternativas

---

## Hitos principales

| Fecha | Hito | Descripción |
|-------|------|-------------|
| 22 ago 2024 | ✅ Especificaciones finalizadas | Decisiones técnicas tomadas |
| 10 oct 2024 | 🔧 Prototipos hardware listos | Primeros dispositivos funcionales |
| 10 oct 2024 | 💻 Firmware v1.0 completado | Funcionalidades core implementadas |
| 14 nov 2024 | 📱 App móvil beta | Aplicación Android funcional |
| 5 dic 2024 | 🌐 Dashboard web v1.0 | Sistema web completo |
| 16 ene 2025 | 🧪 Testing completado | Validación integral finalizada |
| 31 ene 2025 | 🚀 Piloto desplegado | Sistema listo para uso operativo |

---

**Desarrollado por Rodrigo Álvarez (@incognia)**  
**Última actualización**: 21 de julio de 2025
