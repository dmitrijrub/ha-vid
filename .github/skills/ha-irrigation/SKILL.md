---
name: ha-irrigation
description: Control garden irrigation and watering. Use when asked to water the garden, turn on/off irrigation zones, check watering schedules, or control the B-Hyve smart irrigation timer. Covers zones like terasa, laselyne, lysve, and others.
---

Use this skill to control and query the garden irrigation system.

## Irrigation System Context

- **Integration:** B-Hyve (Orbit B-Hyve smart irrigation timer)
- **Account:** mesnic@gmail.com
- **Entity type:** `valve` (6 zones)

## Zone Mapping

| Zone name | Description |
|---|---|
| Bonzo medis zone | Tree/shrub zone (Bonzo tree area) |
| Terasa zone | Terrace/patio zone |
| Terasa kampe zone | Terrace corner zone |
| Prie lauko bloku zone | Near outdoor unit zone |
| Laselyne zone | Drip irrigation zone |
| Pakelta Lysve zone | Raised garden bed zone |

Search for all zones: `ha_search_entities` with `domain_filter="valve"` and `query="Smart Indoor Timer"`.

## Workflow

### To turn on an irrigation zone:
1. Identify the zone from user input using the zone mapping above.
2. Call `ha_search_entities` with `domain_filter="valve"` and the zone name.
3. Call `ha_call_service` with `valve.open_valve` and the resolved `entity_id`.
4. Verify with `ha_get_state` — state should be `open`.
5. Remind the user to close the valve after watering, or ask for a duration.

### To turn off an irrigation zone:
1. Resolve the valve entity.
2. Call `ha_call_service` with `valve.close_valve`.
3. Verify state becomes `closed`.

### To check which zones are active:
1. Call `ha_search_entities` with `domain_filter="valve"` and `query="Smart Indoor Timer"`.
2. Call `ha_get_state` on all found valve entities.
3. Report which zones are open (watering) and which are closed.

### To turn off all zones:
1. Get all valve entities.
2. Use `ha_bulk_control` or call `valve.close_valve` for each open valve.

### To check B-Hyve integration status:
1. Use `ha_get_state` on the B-Hyve sensor/device entities to verify connectivity.

## Rules
- Always confirm before opening multiple zones simultaneously (may exceed water pressure).
- If the user doesn't specify a zone, ask which zone or list available zones.
- Valves in `closed` state are not watering — `open` means actively watering.
- If a zone has been open for an unusually long time, warn the user.
- Do not open valves if outdoor temperature is below freezing — warn the user.
