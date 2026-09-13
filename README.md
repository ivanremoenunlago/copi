# ExcellentJobs Migration Notes

Este proyecto recoge la migración y personalización de la configuración de ExcellentJobs desde el formato legacy v1.x al formato moderno v2.x usado por el plugin actual.

## Objetivo
Mantener los jobs personalizados del servidor, migrarlos a la estructura actual del plugin y dejar únicamente lo que realmente interesa para el funcionamiento del servidor.

## Estado actual de la configuración

### Módulos activos
En [ExcellentJobs/engine.yml](ExcellentJobs/engine.yml) dejamos activados estos módulos:

- Grind: true
- Leveling: true
- Stats: true
- Contracts: false
- Zones: false

Esto deja el sistema funcional para ganar XP, subir nivel y recibir recompensas, sin activar sistemas extra como contratos ni zonas.

### Progresión
La progresión global del plugin quedó configurada en modo geométrico para seguir la lógica anterior:

```yml
Progression:
  Type: geometric
  Reset-On-Leave: false
  Settings:
    geometric:
      Base: 300.0
      Multiplier: 1.0584421
```

Esto es equivalente a la lógica legacy basada en:

- XP_Initial: 300
- XP_Factor: 1.0584421

### Reward pools
Los rewards se gestionan desde la carpeta [ExcellentJobs/level_rewards](ExcellentJobs/level_rewards).

Los pools creados para la progresión por niveles son:

- money_f.yml -> 50 por nivel
- money_e.yml -> 250 por nivel
- money_d.yml -> 1250 por nivel
- money_c.yml -> 10000 por nivel
- money_b.yml -> 75000 por nivel
- money_a.yml -> 500000 por nivel

Cada job utiliza el pool económico de su rango y un pool específico (`*_rewards`) para entregar sus permisos mediante comandos `settrack`.

Estos han sido ajustados para activarse cada 10 niveles:

- 10, 20, 30, 40, 50, 60, 70, 80, 90, 100

Y en la configuración global también queda la entrega automática:

```yml
Rewards:
  Claim-Required: false
```

## Jobs migrados personalizados
Los jobs que quedaron preservados en la nueva estructura son:

- agriculturist
- armorer
- builder
- digger
- enchanter
- lumberjack
- master_blacksmith
- miner
- toolsmith
- weaponsmith

Cada job tiene su definición en [ExcellentJobs/jobs](ExcellentJobs/jobs), y sus objetivos asociados en [ExcellentJobs/objectives](ExcellentJobs/objectives).

## Jobs con permiso requerido
Siguiendo la lógica de la configuración vieja, los jobs abiertos solo para todos son:

- agriculturist
- digger
- miner

Los siguientes quedaron con permiso requerido:

- armorer
- builder
- enchanter
- lumberjack
- master_blacksmith
- toolsmith
- weaponsmith

Esto se controla con:

```yml
Behavior:
  Permission-Required: true|false
```

## Jobs eliminados por defecto
Se borraron los jobs que eran placeholders o defaults del plugin y no formaban parte de la configuración personalizada del servidor.

## Objetivos
Los objetivos se han migrado a la arquitectura moderna del plugin, separando cada tarea en archivos individuales, por ejemplo:

- agriculturist_break_block.yml
- agriculturist_fertilizing.yml
- agriculturist_gathering.yml
- agriculturist_mining.yml
- builder_building.yml
- armorer_cooking.yml
- armorer_forging.yml
- armorer_crafting.yml
- miner_mining.yml
- miner_gathering.yml
- etc.

## Base de datos
El archivo [ExcellentJobs/engine.yml](ExcellentJobs/engine.yml) se dejó preparado para MySQL con la configuración vieja, porque la base de datos original del servidor estaba en MySQL y no en SQLite.

La clave es mantener la configuración MySQL antigua si se quiere conservar la continuidad del progreso del servidor.

## Notas importantes
- La configuración nueva usa el formato v2.x del plugin y no el legacy v1.x.
- Las estructuras antiguas (jobs con `XP_Initial`, `XP_Factor`, `Max_Level`, `Rewards.List`) no son directamente compatibles, por eso se migraron a la nueva estructura basada en `Progression`, `Objective-Ids` y `Reward-Pool-Ids`.
- Los archivos del antiguo backup se conservan en [ExcellentJobs_old](ExcellentJobs_old) como referencia histórica.

## Estructura del proyecto

- [ExcellentJobs](ExcellentJobs) — configuración del plugin actual
- [ExcellentJobs_old](ExcellentJobs_old) — backup del plugin legacy
- [README.md](README.md) — documentación del proyecto

## Recomendación final
Para un servidor funcional y limpio, la configuración actual queda enfocada en:

- jobs personalizados
- progresión geométrica
- rewards automáticos por nivel
- grind activo
- leveling activo
- stats activo
- contracts y zones desactivados

Esto permite jugar con jobs y progresión sin activar mecánicas extra que no se usan en el servidor.
