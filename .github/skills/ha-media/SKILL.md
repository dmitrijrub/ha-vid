---
name: ha-media
description: Control TVs, speakers, and media players. Use when asked to turn on/off TV, play music, change volume, change source/input, pause, or check what's playing. Covers Samsung TVs, Denon AVR, HEOS speakers, and Spotify.
---

Use this skill to control and query media players throughout the home.

## Media System Context

| Device | Room | Type |
|---|---|---|
| svetaine (QE65QN90AATXXH) | Svetainė (living room) | Samsung TV |
| virtuve (QE50LS03AAUXXH) | Virtuvė (kitchen) | Samsung TV |
| miegamasis (UE55AU9072UXXH) | Miegamasis (bedroom) | Samsung TV |
| Denon Centras | Central / living room | Denon AVR receiver |
| Denon Home 150 Sunaus kamb. | Sūnaus kambaris | HEOS speaker |
| Denon Home 150 Dukros kamb. | Dukros kambaris | HEOS speaker |
| Spotify | Whole home | Spotify Connect |
| Telia TV 650 | — | Android TV (ignored) |

## Workflow

### To turn on/off a TV or device:
1. Identify the device/room from user input.
2. Call `ha_search_entities` with `domain_filter="media_player"` and the room or device name.
3. Call `ha_call_service`:
   - On: `media_player.turn_on`
   - Off: `media_player.turn_off`
4. Verify with `ha_get_state`.

### To change volume:
1. Resolve the media player entity.
2. Call `ha_call_service` with `media_player.volume_set` and `data={"volume_level": VALUE}`.
   - `volume_level` is 0.0–1.0 (e.g., 0.3 = 30%).
   - Or use `media_player.volume_up` / `media_player.volume_down` for relative changes.

### To change source/input:
1. Get current sources with `ha_get_state` — check `attributes.source_list`.
2. Call `ha_call_service` with `media_player.select_source` and `data={"source": "SOURCE_NAME"}`.

### To play/pause/stop:
1. Resolve the entity.
2. Call `ha_call_service`:
   - `media_player.media_play`
   - `media_player.media_pause`
   - `media_player.media_stop`

### To check what's playing:
1. Call `ha_get_state` on the relevant entity (or all media players if unspecified).
2. Report: state, `attributes.media_title`, `attributes.media_artist`, `attributes.volume_level`, `attributes.source`.

### Spotify:
- Use `ha_search_entities` with `query="Spotify"` and `domain_filter="media_player"`.
- Supports standard play/pause/volume/source controls.

## Rules
- If the user says "TV" without a room, ask which room.
- If a media player state is `unavailable`, report it — the device may be powered off or unreachable.
- Samsung TVs may need to be powered on via their remote/WakeOnLAN before responding to commands.
- Volume should always be expressed as a percentage to the user (multiply by 100).
