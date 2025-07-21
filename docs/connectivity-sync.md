# Sistema de conectividad y sincronización autónoma - Polilop v2.0

## Principios de diseño

El dispositivo Polilop v2.0 está diseñado para operar de forma **completamente autónoma**, sin requerir conexión a PC, tablet u otros dispositivos para la sincronización de datos. Todo el proceso de subida de información se realiza automáticamente mediante conectividad de red.

## Arquitectura de conectividad

### Jerarquía de conexión (prioridad descendente)

1. **WiFi conocidas** - Conexión preferida por menor costo
2. **WiFi abiertas** - Solo redes públicas confiables preconfiguradas
3. **5G celular** - Máxima velocidad cuando está disponible
4. **4G LTE** - Fallback estándar para la mayoría de ubicaciones
5. **3G UMTS** - Última opción para zonas remotas

### Gestión inteligente de conectividad

```mermaid
graph TD;
    A[Scan WiFi Networks] --> B[Test Connection Quality];
    B --> C[Evaluate Signal & Cost];
    C --> D{Decision};
    D -->|Use Cellular| E[Cellular Connection (5G/4G/3G)];
    D -->|Use WiFi| F[WiFi Connection (Known/Open)];
```

## Almacenamiento local y cola de sincronización

### Estructura de datos local (SQLite recomendado)

```sql
-- Tabla principal de escaneos
CREATE TABLE scans (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    scan_uuid TEXT UNIQUE NOT NULL,
    device_id TEXT NOT NULL,
    barcode_data TEXT NOT NULL,
    barcode_type TEXT NOT NULL,
    timestamp_utc TEXT NOT NULL,
    latitude REAL,
    longitude REAL,
    sync_status INTEGER DEFAULT 0, -- 0=pending, 1=synced, 2=error
    retry_count INTEGER DEFAULT 0,
    created_at TEXT DEFAULT CURRENT_TIMESTAMP,
    synced_at TEXT
);

-- Tabla de configuración de redes
CREATE TABLE wifi_networks (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    ssid TEXT NOT NULL,
    password TEXT,
    priority INTEGER DEFAULT 0,
    trusted BOOLEAN DEFAULT FALSE,
    last_used TEXT
);

-- Tabla de logs de sincronización
CREATE TABLE sync_logs (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    scan_id INTEGER,
    attempt_time TEXT,
    connection_type TEXT,
    status TEXT,
    error_message TEXT,
    response_code INTEGER,
    FOREIGN KEY (scan_id) REFERENCES scans (id)
);
```

### Estados de sincronización

- **PENDING (0)**: Registro pendiente de sincronización
- **SYNCED (1)**: Registro sincronizado exitosamente
- **ERROR (2)**: Error en sincronización, requiere reintento
- **BLOCKED (3)**: Error crítico, requiere intervención manual

## Protocolo de sincronización

### Endpoint API REST

**URL base**: `https://api.correos.gob.mx/polilop/v2/`

**Endpoints principales**:
- `POST /scans` - Subida de escaneos
- `GET /device/{device_id}/config` - Configuración del dispositivo
- `POST /device/{device_id}/heartbeat` - Señal de vida
- `GET /status` - Estado del servicio

### Formato de datos de sincronización

```json
{
  "device_id": "POLILOP-001-ABC123",
  "sync_batch": {
    "timestamp": "2025-01-20T15:30:00Z",
    "scans": [
      {
        "scan_uuid": "550e8400-e29b-41d4-a716-446655440000",
        "barcode_data": "123456789012",
        "barcode_type": "EAN13",
        "timestamp_utc": "2025-01-20T15:25:30Z",
        "location": {
          "latitude": 19.4326,
          "longitude": -99.1332,
          "accuracy": 5.0
        },
        "metadata": {
          "signal_strength": -67,
          "battery_level": 85,
          "firmware_version": "2.0.1"
        }
      }
    ]
  }
}
```

### Autenticación y seguridad

- **Certificados TLS**: Solo conexiones HTTPS
- **API Key por dispositivo**: Cada dispositivo tiene credenciales únicas
- **Token JWT**: Renovación automática cada 24 horas
- **Validación de integridad**: Hash SHA-256 de datos críticos

```json
{
  "headers": {
    "Authorization": "Bearer jwt-token-here",
    "X-Device-ID": "POLILOP-001-ABC123",
    "X-API-Key": "device-specific-api-key",
    "Content-Type": "application/json",
    "X-Data-Hash": "sha256-hash-of-payload"
  }
}
```

## Estrategias de reintento y resilencia

### Algoritmo de backoff exponencial

```python
def calculate_retry_delay(retry_count):
    """
    Calcula el delay para reintento con backoff exponencial
    """
    base_delay = 30  # 30 segundos base
    max_delay = 3600  # Máximo 1 hora
    
    delay = min(base_delay * (2 ** retry_count), max_delay)
    # Agregar jitter aleatorio ±25%
    jitter = delay * 0.25 * (random.random() - 0.5)
    
    return int(delay + jitter)
```

### Límites y umbrales

- **Intentos máximos por registro**: 10 intentos
- **Tiempo máximo de retención local**: 30 días
- **Batch size máximo**: 50 registros por sincronización
- **Timeout de conexión**: 30 segundos
- **Timeout de respuesta**: 60 segundos

### Gestión de memoria y almacenamiento

- **Límite de registros locales**: 10,000 registros
- **Rotación automática**: Elimina registros sincronizados > 7 días
- **Compresión de datos**: GZIP para batches > 1MB
- **Verificación de integridad**: Checksum de base de datos al arranque

## Monitoreo y diagnósticos

### Métricas locales del dispositivo

- Número de registros pendientes de sincronización
- Última sincronización exitosa (timestamp)
- Calidad de señal de red actual
- Tipo de conexión activa
- Número de reintentos fallidos en las últimas 24h

### Pantalla de estado de conectividad

```
┌─────────────────────────────┐
│  📶 CONECTIVIDAD            │
├─────────────────────────────┤
│ Red: Telcel 4G              │
│ Señal: ████░ 75%            │
│ Pendientes: 23 registros    │
│ Última sync: 14:32          │
│ Estado: 🟢 Sincronizando    │
└─────────────────────────────┘
```

### Estados de conectividad visuales (LED RGB)

- 🔴 **Verde**: Conectado y sincronizando
- 🔡 **Amarillo**: Conectado pero con errores ocasionales
- 🔠 **Naranja**: Conectividad intermitente
- 🔴 **Rojo**: Sin conectividad, modo offline
- ⚪ **Blanco**: Buscando redes disponibles

*Nota: El LED verde de encendido indica también el estado de la batería:*
- *Sólido: Batería >10%*
- *Parpadeando rápido: Batería <10%*
- *Parpadeando lento: Cargando*
- *Sólido brillante: Carga completa*

## Configuración de redes WiFi

### Interfaz de usuario para gestión WiFi

```
┌─────────────────────────────┐
│  📶 REDES WIFI              │
├─────────────────────────────┤
│ ► CorreosWiFi    🔒 ████░   │
│   OfficeDomain   🔒 ███░░   │
│   PublicNet      🔓 ██░░░   │
│                            │
│ [Agregar Red]  [Escanear]   │
└─────────────────────────────┘
```

### Proceso de configuración WiFi

1. **Escaneo automático**: Cada 5 minutos cuando no hay conectividad
2. **Conexión automática**: A redes conocidas según prioridad
3. **Configuración manual**: Via interfaz del dispositivo
4. **Configuración remota**: Via API cuando hay conectividad celular

## Consideraciones de seguridad

### Datos en tránsito
- Encriptación TLS 1.3 obligatoria
- Validación de certificados del servidor
- Pinning de certificados para prevenir MITM

### Datos en reposo
- Encriptación AES-256 de la base de datos local
- Credenciales almacenadas en secure element del ESP32
- Rotación automática de claves de cifrado

### Auditoría
- Log de todas las conexiones de red
- Registro de intentos de acceso no autorizados
- Reporte automático de incidentes de seguridad

---

*Diseño de conectividad autónoma por Rodrigo Álvarez (@incognia) - Enfoque en confiabilidad y autonomía operativa*
