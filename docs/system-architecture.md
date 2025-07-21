# Arquitectura del sistema Polilop v2.0

## Arquitectura general del sistema

```mermaid
graph TB
    subgraph "Dispositivo Polilop"
        A[Scanner de Códigos]
        B[ESP32 MicroPython]
        C[GPS Module]
        D[Módulo Celular 5G/4G/3G]
        E[WiFi/Bluetooth]
        F[Memoria SD SQLite]
        G[LEDs + Buzzer]
        
        A --> B
        C --> B
        B --> D
        B --> E
        B --> F
        B --> G
    end
    
    subgraph "Conectividad"
        H[WiFi Networks]
        I[Cellular Networks]
        
        E --> H
        D --> I
    end
    
    subgraph "Aplicación de configuración"
        J[App Android]
        K[Web PWA]
        
        E -.-> J
        E -.-> K
    end
    
    subgraph "Servidor central"
        L[API REST Server]
        M[Sistema Web Admin]
        N[Dashboard/Reportes]
        
        L --> M
        M --> N
    end
    
    subgraph "Base de datos"
        O[(PostgreSQL)]
        P[(PostGIS)]
        
        O --> P
        L --> O
    end
    
    H --> L
    I --> L
    
    style A fill:#e1f5fe
    style B fill:#f3e5f5
    style L fill:#e8f5e8
    style O fill:#fff3e0
    style M fill:#fce4ec
```

## Esquema de base de datos PostgreSQL

```mermaid
erDiagram
    DEVICES ||--o{ SCANS : produces
    DEVICES ||--o{ DEVICE_CONFIGS : has
    SCANS ||--o{ SCAN_EVENTS : contains
    POSTAL_OFFICES ||--o{ SCANS : origin
    DELIVERY_ZONES ||--o{ SCANS : destination
    CARRIERS ||--o{ SCANS : assigned_to
    
    DEVICES {
        uuid device_id PK
        string device_name
        string firmware_version
        string serial_number
        timestamp created_at
        timestamp last_seen
        boolean active
        json hardware_info
    }
    
    SCANS {
        uuid scan_id PK
        uuid device_id FK
        string barcode_data
        string barcode_type
        timestamp scan_timestamp_utc
        decimal latitude
        decimal longitude
        decimal accuracy
        string scan_location_type
        uuid postal_office_id FK
        uuid delivery_zone_id FK
        uuid carrier_id FK
        string package_status
        timestamp created_at
        timestamp synced_at
        boolean is_synced
    }
    
    SCAN_EVENTS {
        uuid event_id PK
        uuid scan_id FK
        string event_type
        timestamp event_timestamp_utc
        decimal latitude
        decimal longitude
        json metadata
        string status
    }
    
    POSTAL_OFFICES {
        uuid office_id PK
        string office_name
        string office_code
        string address
        decimal latitude
        decimal longitude
        string state
        string municipality
        boolean active
    }
    
    DELIVERY_ZONES {
        uuid zone_id PK
        string zone_name
        string zone_code
        geometry zone_boundary
        uuid postal_office_id FK
        decimal avg_delivery_time_hours
    }
    
    CARRIERS {
        uuid carrier_id PK
        string employee_id
        string first_name
        string last_name
        string phone
        uuid assigned_office_id FK
        boolean active
        timestamp created_at
    }
    
    DEVICE_CONFIGS {
        uuid config_id PK
        uuid device_id FK
        string config_type
        json config_data
        timestamp created_at
        timestamp applied_at
        boolean is_active
    }
```

## Flujo de datos para seguimiento de entregas

```mermaid
sequenceDiagram
    participant C as Cartero
    participant D as Dispositivo Polilop
    participant API as API Server
    participant DB as PostgreSQL
    participant WEB as Sistema Web
    
    Note over C,D: En la oficina postal
    C->>D: Escanea paquete para salida
    D->>D: Registra: timestamp_salida + GPS_oficina
    D->>API: POST /scans (status: "OUT_FOR_DELIVERY")
    API->>DB: INSERT scan_event (type: "DEPARTURE")
    
    Note over C,D: Durante el recorrido
    D->>D: Monitoreo GPS continuo
    
    Note over C,D: En punto de entrega
    C->>D: Escanea paquete para entrega
    D->>D: Registra: timestamp_entrega + GPS_destino
    D->>API: POST /scans (status: "DELIVERED")
    API->>DB: INSERT scan_event (type: "DELIVERY")
    
    Note over API,DB: Cálculo automático
    API->>DB: CALCULATE delivery_time = timestamp_entrega - timestamp_salida
    DB->>API: delivery_time_minutes
    
    Note over WEB: Dashboard en tiempo real
    WEB->>API: GET /reports/delivery-times
    API->>DB: SELECT avg_delivery_times, heatmaps
    DB->>API: Datos agregados
    API->>WEB: JSON con métricas
    WEB->>WEB: Actualiza dashboard
```

## Arquitectura de la aplicación web de administración

```mermaid
graph TB
    subgraph "Frontend Web"
        A[Dashboard Principal]
        B[Mapa de Entregas]
        C[Reportes y Métricas]
        D[Gestión de Dispositivos]
        E[Configuración de Usuarios]
        
        A --> B
        A --> C
        A --> D
        A --> E
    end
    
    subgraph "Backend API"
        F[Authentication Service]
        G[Device Management]
        H[Scan Processing]
        I[Reports Generator]
        J[Real-time Notifications]
        
        F --> G
        F --> H
        F --> I
        F --> J
    end
    
    subgraph "Servicios Auxiliares"
        K[Redis Cache]
        L[File Storage]
        M[Email Service]
        N[WebSocket Server]
        
        G --> K
        I --> L
        J --> M
        J --> N
    end
    
    subgraph "Base de Datos"
        O[(PostgreSQL)]
        P[(PostGIS Extensions)]
        Q[Time Series Tables]
        
        O --> P
        O --> Q
    end
    
    A -.-> F
    B -.-> H
    C -.-> I
    D -.-> G
    E -.-> F
    
    G --> O
    H --> O
    I --> O
    
    style A fill:#e3f2fd
    style F fill:#f1f8e9
    style O fill:#fff8e1
    style K fill:#fce4ec
```

## Aplicación de configuración del dispositivo

```mermaid
graph TB
    subgraph "Aplicación Android/PWA"
        A[Pantalla de Conexión BLE]
        B[Dashboard del Dispositivo]
        C[Config WiFi]
        D[Config Celular]
        E[Actualización Firmware]
        F[Diagnósticos]
        
        A --> B
        B --> C
        B --> D
        B --> E
        B --> F
    end
    
    subgraph "Dispositivo Polilop"
        G[Bluetooth LE Service]
        H[WiFi Manager]
        I[Cellular Manager]
        J[Firmware Updater]
        K[System Diagnostics]
        
        G --> H
        G --> I
        G --> J
        G --> K
    end
    
    subgraph "Servicios GATT BLE"
        L[WiFi Configuration Service]
        M[Cellular Config Service]
        N[Firmware Update Service]
        O[Diagnostics Service]
        
        H --> L
        I --> M
        J --> N
        K --> O
    end
    
    C -.->|BLE| L
    D -.->|BLE| M
    E -.->|BLE| N
    F -.->|BLE| O
    
    style A fill:#e8f5e8
    style G fill:#e1f5fe
    style L fill:#fff3e0
```

## Flujo de configuración inicial del dispositivo

```mermaid
sequenceDiagram
    participant T as Técnico
    participant A as App Config
    participant D as Dispositivo
    participant API as Servidor API
    
    T->>D: Activa modo configuración (botón 3 seg)
    D->>D: Genera PIN aleatorio
    D->>D: Muestra PIN via LEDs (morse)
    
    T->>A: Abre app de configuración
    A->>A: Escanea dispositivos BLE
    A->>T: Muestra "Polilop-ABC123"
    T->>A: Selecciona dispositivo
    A->>T: Solicita PIN
    T->>A: Introduce PIN observado
    A->>D: Intenta conexión BLE + PIN
    D->>A: Conexión establecida
    
    A->>D: Lee estado actual del dispositivo
    D->>A: Retorna config actual
    A->>T: Muestra dashboard del dispositivo
    
    T->>A: Configura redes WiFi
    A->>D: Envía configuración WiFi
    D->>D: Almacena config WiFi
    D->>A: Confirma guardado
    
    T->>A: Actualiza firmware (opcional)
    A->>D: Inicia update de firmware
    D->>API: Descarga nuevo firmware
    API->>D: Transfiere firmware
    D->>D: Verifica e instala
    D->>A: Confirma instalación
    
    T->>A: Finaliza configuración
    A->>D: Desconecta BLE
    D->>D: Sale de modo configuración
    D->>D: Reinicia servicios normales
```

---

*Diagramas de arquitectura por Rodrigo Álvarez (@incognia)*
