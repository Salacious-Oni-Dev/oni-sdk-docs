# Art wanted: building animations

*Oxygen Not Included: now with actual reasons for the oxygen to be not included.*

Many of the SDK's new buildings need their own animations (kanims). Most work today with a
borrowed stand-in from a vanilla building. If you make Oxygen Not Included-style building art,
this list shows what is needed, in priority order. P0 is the most wanted.

To offer art or claim a row, open an [idea request](https://github.com/Salacious-Oni-Dev/oni-sdk-docs/issues/new?template=idea-request.yml)
naming the row number. Art that ships is credited to its artist in the release notes.

## What one building needs

A `place` anim (the build preview), `off`, `on`, a `working_loop` for anything with a duty cycle,
and a `ui` icon. `working_pre`/`working_pst` and `broken` are only needed where a row says so.

## Priority bands

- **P0:** the refrigeration loop (nothing in the physics mod can be shown without these).
- **P1:** heat rejection: radiators, exchanger sizes, wall heater, Stirling engine.
- **P2:** valves, pumps, vents, phase-change vessels, tanks and instruments.
- **P3:** automation.
- **P4:** atmosphere processing and planetary logistics.
- **unranked:** buildings added after the bands were set.

## The list

This is a running list, updated at every alpha. A building gets a row when it is designed, and
its status moves from "needed" to "in progress" (someone has claimed it) to "done" (the art ships
in a release). Rows marked "stand-in art" are buildings you can already build in the game,
wearing a vanilla building's animation. Gaps in the numbering are rows merged into others.

| # | Building | Priority | Art status |
|---|---|---|---|
| 1 | Heat Exchanger | P0 | needed |
| 2 | Large Heat Exchanger | P1 | needed |
| 3 | Counterflow Heat Exchanger | P1 | needed |
| 4 | Convection Radiator (pipe-mounted) | P1 | needed (building works with stand-in art) |
| 5 | Vacuum Radiator (flat panel) | P1 | needed (building works with stand-in art) |
| 6 | Medium Convection Radiator | P1 | needed |
| 7 | Medium Radiator (vacuum) | P1 | needed |
| 8 | Large Extendable Radiator | P1 | needed |
| 9 | Expansion Valve | P0 | needed |
| 10 | Condensation Valve | P0 | needed |
| 11 | Pressure Regulator | P2 | needed |
| 12 | Back Pressure Regulator | P2 | needed |
| 13 | Purge Valve | P0 | needed |
| 14 | Check Valve (one-way) | P2 | needed |
| 15 | Digital Valve | P2 | needed |
| 16 | Pressurant Valve | P2 | needed |
| 17 | In-Line Pump (gas / liquid) | P2 | needed |
| 18 | Turbo In-Line Pump | P2 | needed |
| 19 | Active Vent | P2 | needed (building ships in Alpha 2 with stand-in art) |
| 20 | Passive Vent | P2 | needed |
| 21 | Powered Vent (Large) | P2 | needed |
| 22 | Wall Cooler | P0 | needed |
| 23 | Wall Heater | P1 | needed |
| 24 | Pipe Heater | P2 | needed |
| 25 | Evaporation Chamber | P2 | needed |
| 26 | Condensation Chamber | P2 | needed |
| 27 | In-Line Tank | P2 | needed |
| 28 | Insulated In-Line Tank | P2 | needed |
| 29 | Pressure Vessel | P2 | needed |
| 30 | Pipe Analyzer | P2 | needed |
| 31 | Pipe Meter | P2 | needed |
| 32 | Combustor | P4 | needed |
| 33 | Carbon Sequester | P4 | needed |
| 34 | Reverse Electrolyzer | P4 | needed |
| 35 | Nitrolyzer | P4 | needed |
| 36 | Data Cable | P3 | needed |
| 37 | Data Cable Bridge | P3 | needed |
| 38 | Device Connector | P3 | needed |
| 39 | IC Housing | P3 | needed |
| 40 | IC Housing (Compact) | P3 | needed |
| 43 | Logic Reader | P3 | needed |
| 44 | Logic Writer | P3 | needed |
| 45 | Batch Reader | P3 | needed |
| 46 | Batch Writer | P3 | needed |
| 47 | Slot Reader | P3 | needed |
| 48 | Numeric Memory | P3 | needed |
| 49 | Logic Math | P3 | needed |
| 50 | Logic Compare | P3 | needed |
| 51 | Logic Select / Min-Max | P3 | needed |
| 52 | PID Controller | P3 | needed |
| 53 | Logic Dial | P3 | needed |
| 54 | Button | P3 | needed |
| 55 | LED Display (Small) | P3 | needed |
| 56 | LED Display (Medium / Large) | P3 | needed |
| 57 | Console | P3 | needed |
| 58 | Motherboards (6 item icons, for the Console) | P3 | needed |
| 59 | Weather Station | P4 | needed |
| 61 | Communications Console + Satellite Dish | P4 | needed |
| 62 | Stirling Engine | P1 | needed |
| 63 | Fire Sprinkler | P2 | needed (building ships in Alpha 2 with stand-in art) |
| 64 | Mash Tun | unranked | needed (building ships in Alpha 2 with stand-in art) |
| 65 | Sparker | unranked | needed (building ships in Alpha 2 with stand-in art) |
| 66 | Campfire | unranked | done (ships in Alpha 2) |

Totals: about 39 new kanims, about 25 recolours or faceplate variants, and 6 item icons. Two
family bases (the 1x1 pipe fitting and the 1x1 wall brick) set the look for 18 buildings, so they
come first within their groups.
