# Resumen ejecutivo - Polilop v2.0

## Introducción
Polilop v2.0 es un sistema avanzado de seguimiento postal de código abierto diseñado para Correos de México. Combina innovaciones en hardware y software para mejorar el seguimiento y la gestión de paquetes desde las oficinas postales hasta su entrega final. El sistema soporta una conectividad mejorada, autonomía y análisis de datos, fomentando la colaboración y personalización dentro de una comunidad más amplia.

## Componentes del sistema
1. **Dispositivo escáner autónomo:**
   - Equipado con microcontrolador ESP32, el dispositivo cuenta con conectividad celular 5G/4G/3G, Wi-Fi, Bluetooth, GPS y un escáner de códigos de barras para operación independiente.
   - Soporta actualizaciones y mantenimiento modulares.

2. **Aplicación móvil Android:**
   - Facilita la gestión del dispositivo a través de Bluetooth con características como configuraciones Wi-Fi/celular y actualizaciones de firmware.
   - Diseñada con Material Design 3, soporte para Android 8.0+ en español.

3. **Dashboard administrativo web:**
   - Proporciona una plataforma responsiva para visualización de datos, georreferenciación, mapas de calor y análisis de tiempos de entrega en tiempo real.
   - Utiliza PostgreSQL con PostGIS para la gestión de la base de datos.

## Características técnicas
- **Hardware:**
  - Diseño cilíndrico para operación con una mano con componentes robustos como un sistema de gestión de batería y PCB.
  - Ofrece configuraciones de batería estándar y extendida para uso a largo plazo.

- **Software y conectividad:**
  - Soporta MicroPython en ESP32 con APIs REST para sincronización de datos.
  - Implementa gestión inteligente de energía y protocolos de conectividad superiores.

## Arquitectura del sistema
La arquitectura de Polilop v2.0 integra varios componentes para una operación fluida:
- **Conectividad:** Prioriza redes Wi-Fi y celulares para sincronización de datos.
- **Base de datos:** Emplea SQLite localmente y PostgreSQL/PostGIS para almacenamiento en la nube.

## Gestión de energía
- **Sistema inteligente:**
  - El dispositivo cuenta con un módulo de conectividad eficiente en energía con modos de suspensión para maximizar la vida útil de la batería.
  - Incorpora mecanismos de protección contra fallas para la protección de datos y apagado automático en niveles críticos de batería.

## Diseño modular
- **Escáner de códigos de barras:**
  - Intercambiable con opciones para lectura estándar, de largo alcance, y 2D.
- **Packs de batería:**
  - Ofrece tres configuraciones para diferentes necesidades de uso.

## Conectividad y sincronización
- **Protocolo jerárquico de conexión:**
  - Da prioridad al Wi-Fi por rentabilidad, seguido de redes celulares para máxima cobertura.
- **Almacenamiento local y cola de sincronización:**
  - Recomienda SQLite para gestión de datos y resistencia contra fallos de conectividad.

## Configuración bluetooth
- **Administración remota vía móvil o PWA:**
  - Soporta conexiones seguras Bluetooth Low Energy (BLE) para tareas de configuración.
- **Funciones:**
  - Incluye configuración Wi-Fi, gestión celular y diagnósticos del sistema.

## Conclusiones
Polilop v2.0 está preparado para transformar las operaciones postales con sus capacidades autónomas de seguimiento y una arquitectura de sistema robusta, apoyando configuraciones flexibles e integración sin costuras. El proyecto ejemplifica el potencial de soluciones de código abierto en la administración pública, atendiendo a las necesidades modernas de los servicios postales.

*Desarrollado por Rodrigo Álvarez (@incognia)*  

