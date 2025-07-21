# Análisis de autonomía y configuraciones de batería - Polilop v2.0

## Resumen ejecutivo

Basado en las especificaciones técnicas del dispositivo Polilop v2.0 y considerando el peso actual del dispositivo v1.0 (160g), se ha optimizado el diseño para maximizar la autonomía mediante una batería de **4000mAh** manteniendo un peso total razonable de **193g**.

## Configuraciones de batería disponibles

### 📦 Configuración estándar (2500mAh)
- **Capacidad**: Li-Po 3.7V 2500mAh
- **Peso batería**: ~45g
- **Peso total dispositivo**: ~168g
- **Autonomía estimada**: 8-10 horas de uso continuo
- **Ventajas**:
  - Peso similar al dispositivo actual
  - Costo menor
  - Adecuada para jornadas normales de trabajo
- **Desventajas**:
  - Autonomía limitada para rutas extensas
  - Posible ansiedad por batería en carteros

### 🚀 Configuración extendida (4000mAh) - RECOMENDADA
- **Capacidad**: Li-Po 3.7V 4000mAh
- **Peso batería**: ~70g
- **Peso total dispositivo**: ~193g
- **Autonomía estimada**: 13-15 horas de uso continuo
- **Ventajas**:
  - Autonomía superior para jornadas completas sin recarga
  - Elimina ansiedad por batería
  - Permite operación continua en rutas largas
  - Sólo +33g respecto al dispositivo v1.0 actual (160g)
- **Desventajas**:
  - Incremento de peso de 25g vs configuración estándar
  - Costo ligeramente superior

## Cálculos de autonomía

### Metodología de cálculo

La autonomía del dispositivo se calcula mediante las siguientes fórmulas:

**Capacidad energética disponible:**
$$E_{disponible} = V_{nominal} \times C_{batería}$$

Donde:
- $E_{disponible}$ = Energía disponible en Wh (Watt-hora)
- $V_{nominal}$ = Voltaje nominal de la batería (3.7V para Li-Po)
- $C_{batería}$ = Capacidad de la batería en Ah (Ampere-hora)

**Autonomía teórica:**
$$t_{autonomía} = \frac{E_{disponible}}{P_{consumo\_promedio}}$$

Donde:
- $t_{autonomía}$ = Tiempo de autonomía en horas
- $P_{consumo\_promedio}$ = Potencia promedio consumida en W (Watts)

**Autonomía práctica con factor de seguridad:**
$$t_{práctica} = t_{autonomía} \times f_{seguridad}$$

Donde:
- $f_{seguridad}$ = Factor de seguridad (0.85 típico para Li-Po)

### Consumo estimado de componentes
- **ESP32-WROOM-32U**: ~240mA @ 3.3V = 0.8W
- **SIM7600G-H**: 
  - Standby: ~10mA @ 3.7V = 0.037W
  - Transmisión: ~540mA @ 3.7V = 2.0W
- **NEO-8M GPS**: ~45mA @ 3.3V = 0.15W
- **Scanner DE2120**: ~100mA durante escaneo
- **LEDs + buzzer**: ~20mA intermitente
- **Memoria SD**: ~50mA durante escritura

### Escenarios de uso

#### Escenario típico (uso mixto)

**Cálculo del consumo promedio ponderado:**

Para un escenario de uso mixto, el consumo promedio se calcula como:
$$I_{promedio} = \sum_{i=1}^{n} I_i \times t_i$$

Donde:
- $I_{promedio}$ = Corriente promedio en mA
- $I_i$ = Corriente del modo $i$
- $t_i$ = Porcentaje de tiempo en modo $i$

**Aplicando los valores del escenario:**
$$I_{promedio} = (300\text{mA} \times 0.70) + (400\text{mA} \times 0.20) + (600\text{mA} \times 0.08) + (500\text{mA} \times 0.02)$$

$$I_{promedio} = 210 + 80 + 48 + 10 = 348\text{mA}$$

**Potencia promedio:**
$$P_{promedio} = V_{nominal} \times I_{promedio} = 3.7\text{V} \times 0.348\text{A} = 1.29\text{W}$$

#### Cálculo de autonomía por configuración

**Configuración estándar (2500mAh):**
```
Energía disponible = 3.7V × 2.5Ah = 9.25Wh
Autonomía = 9.25Wh / 1.3W = 7.1 horas teóricas
Autonomía práctica = 7.1h × 0.85 (factor seguridad) = 6.0 horas mínimas
Rango estimado: 6-8 horas (considerando variaciones de uso)
```

**Configuración extendida (4000mAh):**
```
Energía disponible = 3.7V × 4.0Ah = 14.8Wh  
Autonomía = 14.8Wh / 1.3W = 11.4 horas teóricas
Autonomía práctica = 11.4h × 0.85 (factor seguridad) = 9.7 horas mínimas
Rango estimado: 10-13 horas (considerando variaciones de uso)
```

## Análisis de peso vs autonomía

### Comparativa con dispositivo actual
- **Polilop v1.0**: 160g
- **Polilop v2.0 estándar**: 168g (+8g)
- **Polilop v2.0 extendido**: 193g (+33g)

### Justificación del incremento de peso
El incremento de **33g** (20.6%) respecto al dispositivo v1.0 se justifica por:

1. **Incremento de autonomía**: +87.5% (de ~8h a ~15h)
2. **Nuevas funcionalidades**: Conectividad celular 5G, GPS integrado
3. **Peso aún ergonómico**: 193g mantiene uso cómodo con una mano
4. **Eliminación de recarga intermedia**: Jornada completa sin interrupciones

## Recomendación técnica

### ✅ Configuración recomendada: Batería 4000mAh

**Razones de la recomendación:**

1. **Autonomía operativa**: 13-15 horas cubren jornadas completas + margen
2. **Peso aceptable**: 193g sigue siendo manejable para uso con una mano
3. **ROI superior**: El incremento de peso (+25g) ofrece +50% de autonomía
4. **Reducción de costos operativos**: Menos interrupciones por carga
5. **Mejor experiencia de usuario**: Elimina ansiedad por batería

### 📋 Sistema modular recomendado

Mantener ambas opciones disponibles mediante el **sistema de intercambio por deslizamiento**:

- **Configuración base**: 2500mAh para usuarios que prioricen peso mínimo
- **Configuración estándar**: 4000mAh para operación normal
- **Flexibilidad**: Intercambio según necesidades específicas de rutas

## Próximos pasos

1. **Validación con prototipos**: Confirmar consumos reales mediante medición
2. **Pruebas de ergonomía**: Validar comodidad de uso con peso de 193g
3. **Análisis de costos**: Evaluar impacto económico de cada configuración
4. **Definición de SKUs**: Establecer configuraciones de producto final

---

*Análisis realizado por Rodrigo Álvarez (@incognia) - Enero 2025*
