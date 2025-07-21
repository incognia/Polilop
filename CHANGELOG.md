# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [0.3.5] - 2025-07-21

### Added
- Resumen ejecutivo completo del proyecto Polilop v2.0 en español mexicano
- Documento ejecutivo estructurado en 8 secciones principales más conclusiones
- Síntesis comprehensiva de todos los componentes del sistema
- Overview de características técnicas, arquitectura y roadmap del proyecto

## [0.3.4] - 2025-07-21

### Added
- Background section in README.md comparing existing closed-source Polilop device with proposed open-source solution
- Context explaining limitations of current Windows-dependent USB-Micro cable synchronization system
- Comparison highlighting advantages of new wireless connectivity, platform independence, and open-source approach

## [0.3.3] - 2025-07-21

### Changed
- Cleaned up README.md badges, keeping only essential ones (version/status, licensing, tech stack)
- Removed operational badges (timeline, duration, battery life, connectivity specs)
- Removed redundant badges (open hardware/source badges, duplicating license info)
- Organized remaining badges in clear logical groups for better readability
- Streamlined badge section to focus on core project information

## [0.3.2] - 2025-07-21

### Added
- Comprehensive project badges to README.md header for enhanced project presentation
- Project status badges (version, development status, timeline, duration)
- Licensing badges for dual CERN OHL v2.0 and GPL v3.0 licenses
- Open hardware and open source certification badges
- Technology stack badges (ESP32, MicroPython, Android 8.0+, PostgreSQL/PostGIS)
- Device feature badges (autonomous operation, 15-hour battery life, multi-connectivity)
- Intelligent power management badge linking to technical documentation
- Active links from badges to relevant documentation files for easy navigation
- Organized badge layout in logical groups for improved readability

## [0.3.1] - 2025-07-21

### Changed
- Updated project timeline to 2025-2026 dates maintaining 6-month duration
- Synchronized ROADMAP.md with current timeline (Jul 21, 2025 - Jan 17, 2026)
- Updated all phase dates and milestone schedules in ROADMAP.md
- Aligned Gantt chart in README.md with updated project dates
- Updated project status dates to reflect current timeline
- Fixed table formatting issues in milestone tables
- Enhanced chronological consistency between README.md and ROADMAP.md

## [0.3.0] - 2025-07-21

### Added
- Intelligent power management system for connectivity module
- Three-state power management (DORMANT, ACTIVE, STANDBY) for connectivity modules
- Detailed energy consumption specifications and savings calculations
- Configurable timeout parameters for battery optimization
- Power management algorithm with state machine implementation
- Comprehensive documentation of connectivity power management in `docs/power-management.md`

### Changed
- Updated README.md with intelligent connectivity power management information (23:36)
- Enhanced power management documentation with technical specifications (23:37)

## [0.2.2] - 2025-07-20 22:57

### Fixed
- Correct Mermaid diagram syntax in connectivity-sync.md

## [0.2.1] - 2025-07-20 22:51

### Fixed
- Correct LaTeX formula syntax in power management circuit documentation

## [0.2.0] - 2025-07-20 22:44-22:47

### Added
- Comprehensive dual licensing system (CERN OHL v2.0 for hardware, GPL v3.0 for software) (22:44)
- Complete project structure with .gitkeep files (22:46-22:47)
- Project directories for API, database, firmware, hardware components, tests, and tools
- Hardware subdirectories: 3d-models, assembly, BOM, schematics

## [0.1.3] - 2025-07-20 22:20-22:30

### Added
- Battery analysis and calculations documentation (`docs/battery-analysis.md`) (22:20)
- Project roadmap and development timeline (`ROADMAP.md`) (22:30)

## [0.1.2] - 2025-07-20 22:04

### Added
- Hardware design overview documentation (`hardware/README.md`)

## [0.1.1] - 2025-07-20 21:28-21:45

### Added
- Bluetooth configuration protocols (`docs/bluetooth-configuration.md`) (21:28)
- System architecture documentation (`docs/system-architecture.md`) (21:34)
- PCB conceptual diagrams (`hardware/pcb/conceptual-diagram.md`) (21:45)

## [0.1.0] - 2025-07-20 21:03

### Added
- Initial project setup for Polilop v2.0 postal tracking system
- Modular hardware design specifications (`hardware/modular-design.md`)
- Core documentation structure foundation
