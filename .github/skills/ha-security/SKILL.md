---
name: ha-security
description: View camera snapshots and check security camera feeds. Use when asked to show a camera, check who's at the door, view the garage, see what's happening outside, or review camera footage. Covers 6 Hikvision NVR cameras.
---

Use this skill to view and query the home security cameras.

## Camera System Context

The home has **6 cameras** connected via a **Hikvision NVR** (Network Video Recorder) integration. Cameras are named Camera 01 through Camera 06.

Camera entities: `camera.camera_01` through `camera.camera_06`
Snapshot image entities: `image.camera_01_snapshot` through `image.camera_06_snapshot`

## Workflow

### To show a camera snapshot:
1. If the user specifies a camera number or location, resolve it:
   - If the user says a number (e.g., "camera 1"), use `camera.camera_01`.
   - If the user describes a location ("front door", "garage", "outside"), use `ha_search_entities` with `domain_filter="camera"` to list all cameras, then ask the user which camera covers that area (camera-to-location mapping is not known — confirm with the user if unsure).
2. Call `ha_get_camera_image` with the resolved `entity_id`.
3. Display the image and describe what you see.

### To list all cameras:
1. Call `ha_search_entities` with `domain_filter="camera"` and `query=""`.
2. Report the list of available cameras and their current state (`idle`, `streaming`, `unavailable`).

### To check camera state:
1. Call `ha_get_state` on the camera entities.
2. Report state and any relevant attributes.

### To view the latest snapshot image:
1. Use `ha_get_state` on `image.camera_0X_snapshot` entities — the state is the timestamp of the last snapshot.
2. Use `ha_get_camera_image` to fetch a live image.

## Rules
- When displaying an image, describe what you see (people, vehicles, animals, conditions).
- Do not store, share, or transmit camera images beyond the current session.
- If a camera is `unavailable`, report it — the NVR or camera may be offline.
- If the user asks about motion or events, note that event history is managed by the Hikvision NVR — you can only see the current snapshot, not a recording timeline.
- Always confirm before taking any action that could affect the security system configuration.
