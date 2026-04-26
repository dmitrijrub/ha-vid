---
name: ha-covers
description: Control blinds and curtains in the home. Use when asked to open, close, stop, or set position of blinds (užuolaida) or roman blinds (romanetė). Covers all rooms including bedroom, living room, kitchen, office, and children's rooms.
---

Use this skill to control and query window covers (blinds and roman blinds) throughout the home.

## Cover Types & Vocabulary

- **Užuolaida** — motorized curtain/blind (full fabric curtain)
- **Romanetė** — roman blind (pleated, raises/lowers)

## Room → Cover Mapping

| Room | Cover entity (approximate name) |
|---|---|
| Tėvų miegamasis (parents' bedroom) | Miegamojo užuolaida |
| Alexo / Sūnaus kambaris (son's room) | Sūnaus kamb. užuolaida |
| Dukros / Barboros kambaris (daughter's room) | Dukros kamb. užuolaida |
| Svetainė (living room) | Svetainės užuolaida, Svetainės užuolaida prie virtuvės |
| Virtuvė (kitchen) | Virtuvės užuolaida, Virtuvės romanetė |
| Darbo kambaris (office) | Darbo kamb. romanetė |

## Workflow

### To open or close a cover:
1. Identify the room from user input.
2. Call `ha_search_entities` with `domain_filter="cover"` and the room name as query.
3. If multiple covers found in the room (e.g., living room has two), clarify which or control all if user said "all".
4. Call `ha_call_service`:
   - Open: `cover.open_cover`
   - Close: `cover.close_cover`
   - Stop: `cover.stop_cover`
5. Verify with `ha_get_state` — check `state` (`open`, `closed`, `opening`, `closing`).

### To set a specific position:
1. Resolve the cover entity.
2. Call `ha_call_service` with `cover.set_cover_position` and `data={"position": VALUE}`.
   - `0` = fully closed, `100` = fully open.
3. Confirm position with `ha_get_state` checking `attributes.current_position`.

### To check cover state:
1. Search for covers by room.
2. Call `ha_get_state` and report state and position for each.

## Rules
- Always search before acting — never guess entity IDs.
- If the user says a room that has multiple covers, list them and ask which (or confirm "all").
- Covers via KNX may take a moment to reach final position — state during movement will be `opening` or `closing`.
- If a cover is `unavailable`, report it to the user.
- Lithuanian room names may lack diacritics in user input — handle both: `sunaus` = `Sūnaus`, `dukros` = `Dukros`, `virtuve` = `Virtuvė`.
