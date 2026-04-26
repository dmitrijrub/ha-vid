---
name: ha-climate
description: Control thermostats and air conditioning. Use when asked to set temperature, change heating mode, turn on/off AC, check room temperature, or adjust HVAC settings. Covers KNX room thermostats and Gree AC units.
---

Use this skill to control and query the climate (heating and AC) in the home.

## System Context

The home has two climate systems:
- **KNX heating** — room thermostats controlling underfloor/radiator heating. Entity names match room names (e.g., "Darbo", "Svetaine", "Miegamasis").
- **Gree AC** — split AC units in specific rooms. Integration: `gree`.

## Room → Thermostat Mapping (approximate)

| Room | Thermostat name |
|---|---|
| Darbo kambaris (office) | Darbo / Darbo kambarys |
| Dukros / Barboros kambaris (daughter) | Vaikų kambarys 1 (dukros) |
| Svetainė (living room) | Svetaine / Svetainė |
| Tėvų miegamasis (parents' bedroom) | Tevu / Miegamasis |
| Barbora's / son's room | Barbora / Alex |

## Workflow

### To set temperature in a room:
1. Identify the room from user input.
2. Call `ha_search_entities` with `domain_filter="climate"` and the room name.
3. If multiple results, prefer the one matching the room name most closely.
4. Call `ha_call_service` with `climate.set_temperature`, `entity_id`, and `data={"temperature": TARGET}`.
5. Verify with `ha_get_state` — check `attributes.temperature` (target) and `attributes.current_temperature`.

### To change HVAC mode:
1. Resolve the climate entity.
2. Call `ha_call_service` with `climate.set_hvac_mode` and `data={"hvac_mode": MODE}`.
   - Valid modes: `heat`, `cool`, `auto`, `off`, `fan_only`
3. Confirm with `ha_get_state`.

### To check room temperature:
1. Use `ha_search_entities` with `domain_filter="climate"` or `domain_filter="sensor"` (for temperature sensors).
2. Report current temperature (`attributes.current_temperature`) and target (`attributes.temperature`).

### For Gree AC:
1. Search with `ha_search_entities` with `domain_filter="climate"` and `query="Gree"` or room name.
2. Use the same `climate.set_temperature` and `climate.set_hvac_mode` services.

## Rules
- If the user says "heat" without specifying a room, ask which room.
- If the requested temperature seems extreme (< 5°C or > 30°C), confirm before setting.
- Always verify current temperature vs. target after setting.
- Report HVAC mode, current temp, and target temp together for clarity.
