# Circuito de gestión de energía - Especificaciones de hardware

## Descripción general

Este documento define los componentes de hardware específicos necesarios para implementar el sistema de gestión de energía que garantice:

1. **Monitoreo preciso** del nivel de batería
2. **Apagado automático** al 2% de carga
3. **Encendido automático** con batería nueva

## Componentes principales del circuito

### 1. Circuito de monitoreo de batería

#### Divisor de voltaje para ADC
- **Propósito**: Adaptar voltaje de batería Li-Po (4.2V max) a rango ADC ESP32 (3.3V max)
- **Configuración**: Divisor resistivo con factor 0.79 (3.3V/4.2V)

```
VBat (4.2V) ──┬─── R1 (10kΩ) ──┬─── ADC_PIN (GPIO36)
              │                 │
              └─ R2 (39kΩ) ─────┴─── GND
```

**Cálculos:**

**Factor de división:**
$$\text{Factor de división} = \frac{R2}{R1+R2} = \frac{39k}{10k+39k} \approx 0.795$$

**Voltaje ADC máximo:**
$$V_{max,ADC} = V_{bat,max} \times \text{Factor de división} = 4.2V \times 0.795 = 3.34V$$ (dentro del rango)

**Voltaje ADC al 2%:**
$$V_{ADC,2\%} = V_{bat,2\%} \times \text{Factor de división} = 3.40V \times 0.795 \approx 2.70V$$

#### Filtro de estabilización
- **Condensador**: 100nF cerámico entre ADC_PIN y GND
- **Propósito**: Filtrar ruido y estabilizar lecturas
- **Ubicación**: Lo más cerca posible del pin ADC

### 2. Circuito de detección de batería nueva

#### Comparador de voltaje (Método recomendado)
- **Componente**: LM393 dual comparator (bajo consumo: 0.4mA)
- **Función**: Detectar cuando voltaje de batería supera umbral de encendido

```
VBat ────┬──── R3 (100kΩ) ──┬──── IN+ (LM393)
         │                  │
         └──── C1 (1µF) ────┴──── GND

Vref (3.6V) ──── IN- (LM393)

OUT (LM393) ──── R4 (10kΩ) ──── VCC
     │
     └──── WAKE_PIN (GPIO25)
```

**Configuración:**
- **Vref**: 3.6V (equivalente al 10% de batería)
- **Histéresis**: R5 (1MΩ) entre OUT e IN+ para evitar oscilaciones
- **Pull-up**: R4 (10kΩ) para salida del comparador

#### Circuito de retención (Supercondensador)
- **Componente**: Supercondensador 0.1F/5.5V
- **Función**: Mantener funcionamiento del comparador durante cambio de batería

**Cálculo del tiempo de retención:**

En un circuito RC, el tiempo de descarga se rige por la ecuation:
$$V(t) = V_0 \cdot e^{-\frac{t}{RC}}$$

Donde:
- $V(t)$ = Voltaje en el tiempo $t$
- $V_0$ = Voltaje inicial del condensador (4.2V)
- $R$ = Resistencia de carga (≈ 10kΩ del comparador)
- $C$ = Capacidad del supercondensador (0.1F)

**Constante de tiempo:**
$$\tau = RC = 10 \times 10^3 \times 0.1 = 1000\text{ segundos}$$

**Tiempo para caer de 4.2V a 3.6V (umbral mínimo):**
$$t = -\tau \ln\left(\frac{V_{final}}{V_{inicial}}\right) = -1000 \ln\left(\frac{3.6}{4.2}\right) \approx 151\text{ segundos}$$

**Tiempo de retención estimado**: ~2.5 minutos sin batería

```
VBat ──── D1 (Schottky) ──┬──── Supercap (0.1F)
                          │
                          └──── VCC_Detector
```

### 3. Sistema de alimentación modular

#### Conector de batería deslizante
- **Especificación**: JST-PH 2.0mm, 2 pines (VBat, GND)
- **Corriente máxima**: 3A continua
- **Contactos**: Chapado en oro para mejor conductividad
- **Mecánica**: Sistema de rieles con retención por resorte

#### Switch de alimentación principal
- **Componente**: MOSFET P-channel (SI2301)
- **Función**: Control de alimentación desde batería
- **Características**: 
  - VDS: -20V, ID: -2.3A
  - RDS(on): 130mΩ @ VGS=-2.5V
  - Consumo de fuga: <1µA

```
VBat ──── D2 (Schottky) ──── SOURCE (SI2301)
                              │
GATE (SI2301) ──── CTRL_PIN (GPIO26)
                              │
                            DRAIN ──── VCC_Main (3.3V)
```

### 4. Reguladores de voltaje

#### Regulador principal (Operación normal)
- **Componente**: AMS1117-3.3V (LDO)
- **Entrada**: 3.6V - 4.2V (batería Li-Po)
- **Salida**: 3.3V @ 800mA máximo
- **Dropout**: 1.3V típico
- **Eficiencia**: ~75% @ carga completa

#### Regulador auxiliar (Modo hibernación)
- **Componente**: MCP1700-3.3V (ultralow quiescent)
- **Corriente quiescente**: 1.6µA típico
- **Función**: Alimentar únicamente circuito de detección
- **Entrada**: Directa desde batería a través de diodo

### 5. Circuito de carga (Opcional para futuras versiones)

#### Controlador de carga Li-Po
- **Componente**: TP4056 con protección DW01A
- **Corriente de carga**: 1A máximo (programable con resistor)
- **Protección**: Sobrecarga, descarga profunda, cortocircuito
- **Indicadores**: LEDs para estado de carga

## Esquemático completo integrado

```
                    ┌─────────────────────────────────┐
                    │        BATERÍA Li-Po 4000mAh    │
                    │         3.7V Nominal            │
                    └─────────────┬───────────────────┘
                                  │
                    ┌─────────────┴─────── VBat
                    │
              ┌─────┴──── D2 (Schottky BAT54C)
              │
        ┌─────┴──── SOURCE (SI2301 P-MOSFET)
        │           │
        │         GATE ──── GPIO26 (Power Control)
        │           │
        │         DRAIN ──── VCC_Main
        │                    │
        │              ┌─────┴──── AMS1117-3.3V ──── ESP32_VCC
        │              │
        │         ┌────┴──── Monitor Circuit:
        │         │          - R1(10k), R2(39k) divisor
        │         │          - C1(100nF) filter
        │         │          - GPIO36 (ADC)
        │         │
        └─────────┴──── Comparator Circuit:
                         - LM393 comparator  
                         - Supercap backup
                         - GPIO25 (Wake-up)
```

## Lista de componentes (BOM)

### Componentes principales
| Componente | Valor/Modelo | Cantidad | Función |
|------------|-------------|----------|---------|
| R1 | 10kΩ ±1% | 1 | Divisor voltaje (superior) |
| R2 | 39kΩ ±1% | 1 | Divisor voltaje (inferior) |
| R3 | 100kΩ ±5% | 1 | Entrada comparador |
| R4 | 10kΩ ±5% | 1 | Pull-up salida |
| R5 | 1MΩ ±5% | 1 | Histéresis comparador |
| C1 | 100nF X7R | 1 | Filtro ADC |
| C2 | 1µF X7R | 1 | Filtro entrada comparador |
| SC1 | 0.1F/5.5V | 1 | Supercondensador backup |
| D1, D2 | BAT54C Schottky | 2 | Protección/aislamiento |
| U1 | LM393D | 1 | Comparador dual |
| U2 | AMS1117-3.3 | 1 | Regulador principal |
| U3 | MCP1700-3302E | 1 | Regulador auxiliar |
| Q1 | SI2301DS P-MOSFET | 1 | Switch principal |

### Conectores y mecánicos
| Componente | Especificación | Cantidad | Función |
|------------|---------------|----------|---------|
| J1 | JST-PH 2.0mm 2pin | 1 | Conector batería |
| SW1 | Sistema deslizante | 1 | Mecanismo cambio batería |

## Consideraciones de diseño PCB

### Layout recomendado
1. **Separación de circuitos**: 
   - Circuito analógico (ADC) alejado de switching
   - Ground plane continuo bajo componentes críticos
2. **Ruteo de alimentación**:
   - Pistas gruesas para VBat (mínimo 0.5mm)
   - Vías múltiples para conexiones de alimentación
3. **Filtrado**:
   - Condensadores de desacoplamiento cerca de cada CI
   - Ferrite bead en alimentación ADC si es necesario

### Dimensiones estimadas
- **Área total**: 25mm × 35mm
- **Espesor PCB**: 1.6mm estándar
- **Número de capas**: 2 capas suficientes
- **Ubicación**: Integrado en PCB principal del dispositivo

## Validación y pruebas

### Pruebas de diseño
1. **Simulación SPICE**: Verificar comportamiento del comparador
2. **Análisis térmico**: Confirmar disipación de reguladores
3. **EMI/EMC**: Evaluación de interferencias electromagnéticas

### Pruebas funcionales
1. **Precisión ADC**: Verificar conversión voltaje-porcentaje
2. **Tiempo de respuesta**: Medir latencia de detección de batería
3. **Consumo en hibernación**: Confirmar <10µA en deep sleep
4. **Durabilidad mecánica**: 1000 ciclos de inserción/extracción

## Alternativas y variantes

### Opción simplificada (Prototipo)
- Eliminar comparador, usar solo polling por software
- Usar regulador único (AMS1117)
- Conexión directa de batería (sin switch MOSFET)

### Opción avanzada (Producción)
- Fuel gauge dedicado (MAX17048)
- Controlador de carga integrado
- Comunicación I2C para telemetría avanzada

---

*Diseño de circuito desarrollado por Rodrigo Álvarez (@incognia)*  
*Versión: 1.0 - Enero 2025*
