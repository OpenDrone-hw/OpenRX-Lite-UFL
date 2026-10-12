# OpenRX-Lite-UFL

Open-source 2.4 GHz ExpressLRS receiver: ESP32-C3, SX1281, U.FL antenna connector.

Validate: `kicad-cli sch erc hardware/OpenRX-Lite-UFL.kicad_sch && kicad-cli pcb drc --schematic-parity --refill-zones hardware/OpenRX-Lite-UFL.kicad_pcb`

## Boundaries

- Sourcing (suppliers, prices, quotes, RFQs, contacts) never lives in OpenDrone repositories.
- Never text-edit `.kicad_sch`, `.kicad_pcb` or `.kicad_dru`: use KiCad, or kicad-skip / the pcbnew API with KiCad closed. `.kicad_pro` is JSON and may be edited for metadata.
- Agents write metadata fields only (MPN, Manufacturer, LCSC, Datasheet, text variables), never nets, wiring, routing, placement, footprint assignment or circuit values.
- KiCad libraries stay project-local; import parts with `easyeda2kicad` only.
- KiCad files do not merge: one person holds a board layout at a time.

## Pointers

- Shared parts catalogue: `hardware/KiCad-Library/PARTS-USED.md`
- Release standard: https://github.com/OpenDrone-hw/.github/blob/main/RELEASES.md
