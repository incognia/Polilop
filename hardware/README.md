# Hardware - Polilop v2.0

## Componentes principales

### Diseño físico
- **Forma**: Cilíndrica o cilindroide para uso cómodo con una mano
- **Controles**:
  - Botón pequeño para encendido
  - Botón grande para escaneo
  - Botón oculto para conectividad Bluetooth

### Microcontrolador
- **ESP32-WROOM-32U** (con conector U.FL para antena externa)
- **Memoria**: 520KB SRAM, 4MB Flash
- **Conectividad**: WiFi + Bluetooth integrados
- **GPIO**: 34 pines disponibles para periféricos
- **Bluetooth**: BLE para configuración administrativa y actualización de firmware

### Módulo celular
- **SIM7600G-H** (5G/4G/3G/2G)
- **Bandas soportadas**: 
  - 5G: n1/n2/n3/n5/n7/n8/n20/n25/n28/n38/n40/n41/n66/n71/n77/n78/n79
  - 4G: B1/B2/B3/B4/B5/B7/B8/B12/B13/B17/B18/B19/B20/B25/B26/B28/B66/B71
- **Consumo**: ~2W en transmisión, <10mA en standby

### GPS
- **Módulo**: NEO-8M (integrado en SIM7600G-H)
- **Precisión**: 2.5m CEP
- **Time to first fix**: 26s (cold start)
- **Antena**: Antena GPS cerámica pasiva

### Almacenamiento
- **Memoria SD**: Micro SD hasta 32GB
- **Formato**: FAT32 para compatibilidad
- **Estructura de datos**: SQLite recomendado

### Indicadores y señales
- **LED de encendido/batería (Verde)**:
  - Sólido: Dispositivo encendido, batería >10%
  - Parpadeando rápido: Batería <10%
  - Parpadeando lento: Cargando batería
  - Sólido brillante: Carga completa
  - **Alerta crítica**: 3 beeps agudos cuando batería <5%
- **LED RGB multifunción**: Indica todos los estados del sistema
  - **Estados de escaneo**:
    - Verde: Éxito de escaneo
    - Rojo: Fallo de escaneo
  - **Estados de conectividad**:
    - Azul: Estado Bluetooth
    - Amarillo: Estado WiFi
    - Naranja: Estado celular
- **Buzzer**: Piezo para confirmaciones auditivas
  - Beep agudo para escaneo exitoso
  - Tono grave para fallo de escaneo
  - 3 beeps agudos para batería crítica (<5%)
- **Controles**:
  - Botón pequeño de encendido (lado del dispositivo)
  - Botón grande de escaneo (parte superior)
  - Botón oculto para modo configuración (base del dispositivo)
  - Combinación alternativa: mantener botón de escaneo + encendido por 5 segundos

### Escáner de códigos (Módulo intercambiable)
- **Módulo principal**: DE2120 1D Barcode Scanner Engine
- **Formatos soportados obligatorios**: 
  - Código 39 (Code 39)
  - Código 93 (Code 93)
  - Código 128 (Code 128)
  - Entrelazado 2 de 5 (Interleaved 2 of 5)
  - Codabar
  - UPC-A/UPC-E
  - EAN-8/EAN-13
- **Interface**: UART a 9600 bps
- **Conexión modular**: Conector JST-PH 2.0mm 4 pines (VCC, GND, TX, RX)
- **Dimensiones**: 25mm x 18mm x 12mm
- **Peso**: ~8g
- **Distancia de lectura**: 5cm - 60cm

### Alimentación (Módulo intercambiable)
- **Configuración estándar**: Li-Po 3.7V 2500mAh (pack delgado)
  - **Peso batería**: ~45g
  - **Autonomía estimada**: 8-10 horas de uso continuo
  - **Peso total dispositivo**: ~168g
- **Configuración extendida**: Li-Po 3.7V 4000mAh (pack de alta capacidad) - RECOMENDADA
  - **Peso batería**: ~70g  
  - **Autonomía estimada**: 13-15 horas de uso continuo
  - **Peso total dispositivo**: ~193g
- **Conexión modular**: Conector JST-XH 2.54mm 2 pines con locking
- **Carga**: USB-C PD con IC de gestión TP4056
- **Reguladores**: 
  - 3.3V para ESP32 (AMS1117-3.3 ultra-low dropout)
  - 5V boost converter para módulos periféricos
- **Sistema de intercambio**: Mecanismo de liberación por deslizamiento

## Dimensiones físicas y peso
- **Forma**: Cilíndrica/cilindroide para uso con una mano
- **Dimensiones aproximadas**: 120mm largo x 35mm diámetro
- **PCB principal**: Circular/elíptica adaptada al enclosure cilíndrico
- **PCB escáner**: 30mm x 20mm (2 capas, módulo intercambiable)
- **Enclosure**: Cilíndrico en policarbonato + TPU
- **Peso objetivo**: ≤200g (con batería extendida de 4000mAh)
- **Desglose de peso**:
  - PCB principal + componentes: ~45g
  - Enclosure (carcasa): ~35g
  - Batería Li-Po (configuración estándar 2500mAh): ~45g
  - Batería Li-Po (configuración extendida 4000mAh): ~70g
  - Módulo escáner: ~8g
  - LEDs + botones + buzzer: ~12g
  - Antenas y cables: ~8g
  - Módulos celular/GPS: ~15g
  - **Total configuración estándar**: ~168g (32g margen disponible)
  - **Total configuración extendida**: ~193g (7g margen disponible)

## Conectores y puertos
- **USB-C**: Carga y comunicación
- **SIM**: Nano SIM holder
- **SD**: Micro SD card slot
- **Antenas**: 
  - U.FL para GPS
  - U.FL para celular
  - Antena PCB para WiFi/BT

## Consideraciones ambientales
- **Temperatura operación**: -10°C a +60°C
- **Humedad**: 85% RH máximo
- **Protección**: IP54 (polvo y salpicaduras)

## Esquemáticos

Los archivos de esquemáticos se desarrollarán usando KiCad 6.0+ y se almacenarán en:
- `schematics/polilop-v2.kicad_pro`
- `schematics/polilop-v2.kicad_sch`
- `pcb/polilop-v2.kicad_pcb`

## Lista de materiales (BOM)

Archivo BOM detallado se generará automáticamente desde KiCad y se mantendrá en:
- `bom/polilop-v2-bom.csv`

---

*Documentación técnica de hardware por Rodrigo Álvarez (@incognia)*
