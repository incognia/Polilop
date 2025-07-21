# Diseño modular - Polilop v2.0

## Filosofía de diseño modular

El dispositivo Polilop v2.0 está diseñado con un enfoque modular que permite:
- Mantenimiento rápido y sencillo en campo
- Actualización de componentes específicos sin reemplazar todo el dispositivo
- Reducción de costos de soporte y repuestos
- Flexibilidad para diferentes casos de uso

## Módulos intercambiables

### 1. Módulo lector de códigos de barras

#### Especificaciones del módulo
- **Conector**: JST-PH 2.0mm 4 pines
- **Pinout**:
  - Pin 1: VCC (5V)
  - Pin 2: GND
  - Pin 3: TX (datos del escáner)
  - Pin 4: RX (comandos de configuración)
- **Mecanismo de fijación**: Clip de retención con liberación por presión
- **Tiempo de intercambio**: <30 segundos sin herramientas

#### Variantes disponibles

**Módulo estándar - DE2120**
- Códigos soportados: Todos los requeridos
- Alcance: 5cm - 60cm
- Peso: ~8g
- Consumo: 150mA @ 5V (durante escaneo)

**Módulo de largo alcance - DE2130**
- Códigos soportados: Todos los requeridos + DataMatrix
- Alcance: 10cm - 150cm
- Peso: ~12g
- Consumo: 200mA @ 5V

**Módulo 2D - DE2140**
- Códigos soportados: 1D + QR + DataMatrix + PDF417
- Alcance: 3cm - 40cm
- Peso: ~10g
- Consumo: 180mA @ 5V

#### Proceso de intercambio
1. Apagar el dispositivo
2. Presionar el clip de liberación del módulo escáner
3. Retirar el módulo deslizándolo hacia afuera
4. Insertar el nuevo módulo hasta que haga click
5. Verificar conexión al encender

### 2. Módulo de batería

#### Especificaciones del sistema
- **Conector**: JST-XH 2.54mm 2 pines con sistema de bloqueo
- **Pinout**:
  - Pin 1: BAT+ (3.7V nominal)
  - Pin 2: BAT-/GND
- **Mecanismo de fijación**: Deslizamiento con traba de seguridad
- **Indicador de carga**: LED integrado en el pack

#### Variantes de batería

**Pack estándar - 2500mAh**
- Capacidad: 2500mAh Li-Po
- Dimensiones: 65mm x 45mm x 8mm
- Peso: ~45g
- Autonomía: 8-10 horas uso continuo
- Tiempo de carga: 2.5 horas (USB-C 5V/2A)

**Pack extendido - 4000mAh**
- Capacidad: 4000mAh Li-Po
- Dimensiones: 65mm x 45mm x 12mm
- Peso: ~70g (excede peso objetivo, uso especial)
- Autonomía: 12-15 horas uso continuo
- Tiempo de carga: 4 horas (USB-C 5V/2A)

**Pack de respaldo - 1500mAh**
- Capacidad: 1500mAh Li-Po
- Dimensiones: 55mm x 35mm x 6mm
- Peso: ~28g
- Autonomía: 5-6 horas uso continuo
- Tiempo de carga: 1.5 horas

#### Proceso de intercambio
1. Verificar que el dispositivo esté apagado o conectado a USB-C
2. Deslizar la traba de seguridad hacia la posición "UNLOCK"
3. Deslizar la batería hacia afuera del compartimento
4. Insertar nueva batería hasta que haga click
5. Deslizar la traba a posición "LOCK"

## Sistema de gestión de módulos

### Detección automática
- El firmware detecta automáticamente el tipo de módulo escáner conectado
- Configura automáticamente los parámetros de comunicación
- Muestra en pantalla el modelo detectado durante el arranque

### Configuración por módulo
- Cada tipo de módulo tiene un perfil de configuración específico
- Parámetros como timeout, códigos habilitados, y opciones de escaneo se ajustan automáticamente
- Configuraciones personalizadas se guardan por tipo de módulo

### Sistema de alertas
- Alertas de batería baja con niveles configurables (20%, 10%, 5%)
- Notificación cuando un módulo no es reconocido
- Indicadores LED para estado de módulos

## Herramientas y mantenimiento

### Herramientas requeridas
- **Ninguna**: Todo el intercambio se realiza sin herramientas
- Kit de limpieza óptica para lente del escáner (opcional)

### Programa de mantenimiento preventivo
- **Semanal**: Limpieza de lente del escáner
- **Mensual**: Verificación de conectores
- **Trimestral**: Calibración de batería (ciclo completo de carga/descarga)
- **Anual**: Actualización de firmware y revisión general

### Diagnósticos integrados
- Test de comunicación con módulo escáner
- Verificación de integridad de batería
- Prueba de conectividad celular y GPS
- Reporte de estado de todos los módulos

## Ventajas del diseño modular

### Operativas
- Reducción de tiempo de inactividad por mantenimiento
- Posibilidad de intercambio de módulos en campo
- Flexibilidad para diferentes tipos de códigos según necesidades

### Económicas
- Menor costo de reposición (solo el módulo afectado)
- Aprovechamiento de módulos en buen estado
- Escalabilidad para diferentes presupuestos

### Técnicas
- Facilita actualizaciones tecnológicas parciales
- Permite personalización según el caso de uso
- Simplifica el diagnóstico de fallas

---

*Diseño modular por Rodrigo Álvarez (@incognia) - Enfoque en usabilidad y mantenibilidad para administración pública*
