---
name: ha-home-overview
description: "Get a full status overview of the home: active devices, lights, climate, energy, alerts, and entity counts. Use when asked \"what's happening at home\", \"house status\", \"what's on\", or for a general home summary."
---

Use this skill to give the user a comprehensive, readable summary of the current state of their home.

## House Context

The home has the following rooms (Lithuanian names with English equivalents):
- **Virtuvė** / virtuve (kitchen)
- **Svetainė** / svetaine (living room)
- **Darbo kambaris** (office/work room) — upper right wing
- **Tėvų vonia** (parents' bathroom) — small room, upper right
- **Tėvų miegamasis** (parents' bedroom) — large room, upper right
- **Koridorius / Tambūras** (corridor / entrance hall) — left vertical strip
- **Garažas** (garage) — lower left
- **Skalbykla** (laundry room) — lower left, beside garage
- **WC mažas** (small WC/toilet) — lower centre
- **Dukros / Barboros kambaris** (daughter Barbora's room) — lower centre
- **Alexo / Sūnaus kambaris** (son Alex's room) — lower right centre

Key integrations: KNX (lights, heating), Gree (AC), Samsung TVs, Denon/HEOS audio, Solis solar+battery, Nordpool electricity pricing, Hikvision cameras, Roborock vacuum, B-Hyve irrigation.

## Overview Workflow

1. Call `ha_get_overview` with `detail_level="standard"` to get all entity states grouped by domain.
2. Call `ha_get_system_health` to check for any system issues or repairs needed.
3. Summarize in a friendly format grouped by category:
   - 💡 **Lights** — which rooms have lights on
   - 🌡️ **Climate** — active thermostats and set temperatures
   - 🪟 **Covers** — any open/closed blinds of note
   - 📺 **Media** — any playing devices
   - ⚡ **Energy** — current solar production, battery %, grid import/export
   - 🤖 **Vacuum** — current vacuum state
   - 🚿 **Irrigation** — any active watering zones
   - ⚠️ **Alerts** — unavailable entities, failed integrations, repair items
4. Keep the summary concise. Only mention items that are active or noteworthy.
5. After summarizing, offer to take action: "Would you like me to control anything?"

## Rules
- Do not take any action unless explicitly asked.
- If the user asks to control something after seeing the overview, delegate to the appropriate domain skill.
- If multiple entities are unavailable, list them so the user is aware.
