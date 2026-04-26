---
name: ha-lights
description: Control lights in the home by room or area. Use when asked to turn on/off, dim, brighten, set brightness, or check the state of lights. Handles all rooms including Darbo kambaris, Dukros kambaris, Sūnaus kambaris, Svetainė, Virtuvė, Miegamasis, Koridorius, Garažas, and others.
---

Use this skill to control and query lights throughout the home.

## House Rooms & Aliases

Map user input to the canonical room name before searching:

| User may say | Canonical name |
|---|---|
| darbo, office, work room | Darbo kambaris / darbo kamb |
| dukros, daughter | Dukros kambaris / dukros kamb |
| sunaus, sūnaus, son | Sūnaus kambaris / sunaus kamb |
| svetaine, svetainė, living room, lounge | Svetainė / svetaine |
| virtuve, virtuvė, kitchen | Virtuvė / virtuve |
| miegamasis, bedroom, parents bedroom | Miegamasis |
| koridorius, corridor, hallway | Koridorius |
| garazas, garažas, garage | Garažas |
| barboros, barbora | Barboros kambaris |
| alex | Alex kambaris |
| tamburas, tambūras, entrance | Tambūras |
| lauko, outdoor, outside | Lauko (outdoor lights) |

## Workflow

### To turn lights on or off in a room:
1. Identify the room from user input using the alias table above.
2. Call `ha_search_entities` with `domain_filter="light"` and the room name as the query.
3. If multiple lights are found and the user said "all lights" or did not specify, use `ha_call_service` with `area_id` targeting, or call `ha_bulk_control` for multiple entity IDs.
4. If only one or a specific light is found, call `ha_call_service` with `entity_id`.
5. Verify the result with `ha_get_state`.

### To dim or set brightness:
1. Resolve the entity as above.
2. Call `ha_call_service` with `light.turn_on`, passing `brightness_pct` (0–100) in the `data` field.
3. Confirm the new brightness with `ha_get_state`.

### To check light state:
1. Use `ha_search_entities` with `domain_filter="light"` and the room/name query.
2. Call `ha_get_state` on the found entities.
3. Report which are on, off, or unavailable, with brightness if on.

## Rules
- Always search for entities before acting — never guess entity IDs.
- If you find multiple matching lights and the user didn't specify which, ask for clarification or confirm: "I found X lights in that room, shall I control all of them?"
- If a light is `unavailable`, report it rather than failing silently.
- Prefer `area_id` targeting when controlling all lights in a room.
- KNX lights may respond slightly slower — verify state after a short moment.
