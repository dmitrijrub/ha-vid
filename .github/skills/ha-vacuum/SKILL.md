---
name: ha-vacuum
description: Control the robot vacuum cleaner. Use when asked to start cleaning, stop, pause, send to dock, clean a specific room, or check vacuum status. The home has a Roborock S8 MaxV Ultra.
---

Use this skill to control and query the Roborock S8 MaxV Ultra robot vacuum.

## Vacuum Context

- **Device:** Roborock S8 MaxV Ultra
- **Entity:** `vacuum.s8_maxv_ultra` (search with `ha_search_entities` query="S8" domain_filter="vacuum")
- **Integration:** `roborock`
- **Known rooms/zones (as Roborock segments):**
  - Svetainė (living room)
  - Virtuvė (kitchen)
  - Tėvų miegamasis (parents' bedroom)
  - Darbo kambaris (office)
  - Dukros / Barboros kambaris
  - Alexo / Sūnaus kambaris
  - Koridorius (corridor)
  - Other zones visible as button entities: `button.s8_maxv_ultra_*`

## Workflow

### To start full cleaning:
1. Call `ha_call_service` with `vacuum.start`, `entity_id="vacuum.s8_maxv_ultra"`.
2. Verify with `ha_get_state` — state should change to `cleaning`.

### To stop / send to dock:
1. Stop: `ha_call_service` with `vacuum.stop`.
2. Return to dock: `ha_call_service` with `vacuum.return_to_base`.
3. Verify state becomes `returning` then `docked`.

### To pause:
1. Call `ha_call_service` with `vacuum.pause`.

### To clean a specific room/zone:
1. Check available room buttons: `ha_search_entities` with `query="S8 MaxV Ultra"` and `domain_filter="button"`.
2. Available zone buttons include: "Living & dining area", "Living area", "Pet Area Cleaning", "Full Cleaning".
3. Press the appropriate button: `ha_call_service` with `button.press`, `entity_id=BUTTON_ENTITY`.

### To check vacuum status:
1. Call `ha_get_state` on the vacuum entity.
2. Report: state (`docked`, `cleaning`, `returning`, `paused`, `idle`, `error`), battery level (`attributes.battery_level`).
3. Also check `ha_get_state` on the map entity if the user asks about the cleaning map.

### Do-not-disturb schedule:
- Entities: `time.s8_maxv_ultra_do_not_disturb_begin` and `time.s8_maxv_ultra_do_not_disturb_end`.
- Report or update these with `ha_call_service` using `time.set_value`.

## Rules
- If the vacuum is already cleaning, ask before starting a new cycle.
- If the vacuum reports an error state, describe it clearly.
- Battery level is in `attributes.battery_level` as a percentage.
- Do not run the vacuum during the do-not-disturb window unless explicitly asked.
