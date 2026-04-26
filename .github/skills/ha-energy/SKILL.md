---
name: ha-energy
description: Check solar production, battery status, inverter data, electricity pricing, and energy usage. Use when asked about solar panels, battery charge, Nordpool electricity prices, cheapest hours, grid import/export, or energy cost.
---

Use this skill to query and analyze the home energy system.

## Energy System Context

| Component | Integration | Notes |
|---|---|---|
| Solis solar inverter | `solis` / `solis_cloud_control` | PV production, grid, battery |
| Battery inverter | Solis Cloud Control | Charge/discharge scheduling |
| Nordpool electricity | `nordpool` | Hourly spot prices (Lithuania) |
| Nordpool Planner | `nordpool_planner` | Smart charge scheduling |
| Forecast Solar | `forecast_solar` | Solar production forecast |
| Heat pump (Novelan) | `luxtronik2` | Heat pump energy |
| MyUplink | `myuplink` | Additional device stats |

## Workflow

### To check solar production:
1. Call `ha_search_entities` with `query="solis"` or `query="solar"` and `domain_filter="sensor"`.
2. Key sensors to report: PV power (W/kW), total energy today (kWh), battery voltage, battery SOC (%).
3. Also check: grid import/export power, battery charge/discharge power.

### To check battery status:
1. Search for `query="battery"` in `domain_filter="sensor"`.
2. Report: battery SOC %, voltage, charge/discharge power, and mode.
3. Check inverter control slots with `ha_search_entities` query="inverter controler" for scheduled charge times.

### To check electricity prices (Nordpool):
1. Call `ha_search_entities` with `query="nordpool"` across `sensor` and `select` domains.
2. Key entities:
   - Current price sensor
   - Today's price list / template sensor "Nordpool Prices Today"
   - Average price "Nordpool Average Today"
   - "Nordpool Next Above Average Start"
   - "Nordpool 2h Before Above Average"
3. Present prices in a readable table or summary. Highlight the cheapest and most expensive hours.

### To check energy forecast:
1. Call `ha_get_state` on the Forecast Solar entity.
2. Report expected production for today and tomorrow.

### To check energy history/statistics:
1. Use `ha_get_history` with `source="statistics"` for long-term data.
2. Useful entities: total energy produced (kWh), grid import total, battery throughput.

### To schedule battery charging:
1. Check current inverter slot configuration with `ha_search_entities` query="inverter controler slot".
2. Report current charge/discharge time slots.
3. To modify, use `ha_call_service` with the appropriate text/number entity and target value.
4. Confirm changes by re-reading the slot entity state.

## Rules
- Always express power in W or kW and energy in kWh — be explicit about units.
- Present Nordpool prices in the local currency (ct/kWh or EUR/kWh).
- If asking about "cheap hours", compare each hour's price to the daily average.
- Do not modify inverter settings unless the user explicitly asks.
- For battery SOC, alert if below 10% or if the mode seems unexpected.
