# KiCad 10 migration of TemperatureAdapter

Converted from KiCad 5 with the patched `kicad-cli` from ril3y/kicad-source-mirror
(build/kicad-api-with-protos) and the scripts in synthetos-hw/tools.

## Connectivity check

The original KiCad 5 board is the reference: its pads carry the nets the boards were built with.

```
converted schematic vs original board:
components: reference=16 candidate=16
  missing=[] extra=[] value-changed=[]
multi-pin nets: reference=8 candidate=8
  identical=8 only-in-reference=0 only-in-candidate=0

converted board vs original board:
components: reference=16 candidate=16
  missing=[] extra=[] value-changed=[]
multi-pin nets: reference=8 candidate=8
  identical=8 only-in-reference=0 only-in-candidate=0
```

## Wires rerouted to keep KiCad 5 connectivity

KiCad 5 only connected a pin or wire end that touches the middle of another wire when a
junction was there; KiCad 6+ always connects it. Each through wire below got a 25 mil detour.

None needed.

## KiCad 4 symbol remap

Each symbol's library is the first one in the old project library list that has it;
its drawing comes from the project cache when there was one.

- 22-27-2021 -> _Connector:22-27-2021 (library /Volumes/External/projects/synthetos-hw/gQuintic-TemperatureAdapter/ProjectLibrary/_Connector.lib)
- 22-27-2031 -> _Connector:22-27-2031 (library /Volumes/External/projects/synthetos-hw/gQuintic-TemperatureAdapter/ProjectLibrary/_Connector.lib)
- C -> device:C (library /Volumes/External/projects/synthetos-hw/legacy-libs/kicad-library/library/device.lib)
- D_ZENER -> _Passive:D_ZENER (library /Volumes/External/projects/synthetos-hw/gQuintic-TemperatureAdapter/ProjectLibrary/_Passive.lib)
- Fiducial_Top -> _Connector:Fiducial_Top (library /Volumes/External/projects/synthetos-hw/gQuintic-TemperatureAdapter/ProjectLibrary/_Connector.lib)
- GNDA -> power:GNDA (library /Volumes/External/projects/synthetos-hw/legacy-libs/kicad-library/library/power.lib)
- MCP6002 -> _Active:MCP6002 (library /Volumes/External/projects/synthetos-hw/gQuintic-TemperatureAdapter/ProjectLibrary/_Active.lib)
- R -> device:R (library /Volumes/External/projects/synthetos-hw/legacy-libs/kicad-library/library/device.lib)

## Libraries

- symbol library '_Active': ProjectLibrary/_Active.kicad_sym, converted from ProjectLibrary/_Active.lib; 1 symbols replaced and 0 added from the schematic
- symbol library '_Connector': ProjectLibrary/_Connector.kicad_sym, converted from ProjectLibrary/_Connector.lib; 3 symbols replaced and 0 added from the schematic
- symbol library '_Passive': ProjectLibrary/_Passive.kicad_sym, converted from ProjectLibrary/_Passive.lib; 1 symbols replaced and 0 added from the schematic
- symbol library 'device': ProjectLibrary/device.kicad_sym, rebuilt from the schematic (was not in table); 0 symbols replaced and 2 added from the schematic
- symbol library 'power': ProjectLibrary/power.kicad_sym, rebuilt from the schematic (was not in table); 0 symbols replaced and 1 added from the schematic
- footprint library 'Diodes_SMD': ProjectLibrary/Diodes_SMD.pretty extracted from the board (D_SOD-323)
- footprint library 'Housings_SSOP': ProjectLibrary/Housings_SSOP.pretty extracted from the board (MSOP-8_3x3mm_Pitch0.65mm)

## ERC (all severities)

| severity | type | count |
|---|---|---|
| error | pin_not_connected | 3 |
| error | pin_not_driven | 3 |
| error | power_pin_not_driven | 2 |

## DRC with schematic parity (all severities)

| severity | type | count |
|---|---|---|
| error | clearance | 20 |
| error | copper_edge_clearance | 35 |
| error | courtyards_overlap | 3 |
| error | drill_out_of_range | 8 |
| error | invalid_outline | 1 |
| error | track_width | 123 |
| warning | footprint_symbol_mismatch | 3 |
| warning | net_conflict | 3 |
| warning | silk_edge_clearance | 2 |
| warning | silk_over_copper | 5 |
| warning | silk_overlap | 10 |
| warning | text_height | 24 |
| warning | text_thickness | 21 |
