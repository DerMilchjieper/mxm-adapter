# KiCad layout skeleton

This directory contains a **real KiCad project skeleton** for the RevA MXM-to-OCuLink-x4 concept.

## Files

- `revA-mxm-to-oculink-x4.kicad_pro` — KiCad project settings and placeholder net classes.
- `revA-mxm-to-oculink-x4.kicad_sch` — schematic stub pointing to the documentation-first design.
- `revA-mxm-to-oculink-x4.kicad_pcb` — mechanical/electrical PCB skeleton.

## What is in the PCB file

- 94 mm × 32 mm provisional board outline.
- MXM Type-B edge-finger **placeholder** with 314 pads.
- OCuLink 4i connector **placeholder**.
- Sideband strap-matrix placeholder.
- Preliminary PCIe x4 / REFCLK / sideband net names.
- Drawing-layer high-speed corridor and “do not fabricate” markings.

## What is intentionally not solved yet

- Exact HP ZBook 17 G5 MXM pin mapping.
- Verified MXM edge-card geometry, bevel, plating and insertion depth.
- Verified OCuLink connector footprint from a real vendor part.
- Verified controlled-impedance stackup from a board fabricator.
- Exact lane mapping, polarity, AC-coupling locations and sideband strap values.
- Mechanical retention and cable strain-relief.

## Why the layout is not fabrication-ready

This file is meant to be opened in KiCad to start mechanical review and layout planning. It is **not** a Gerber-ready adapter. The placeholder pad assignments include `_TBD` in net names on purpose. Do not remove that until the ZBook MXM slot has been measured and reviewed.

## Next layout actions

1. Replace the MXM placeholder with a verified edge-card geometry.
2. Replace the OCuLink placeholder with the exact connector vendor footprint.
3. Update `netmap.csv` with measured HP-specific MXM pin mapping.
4. Re-annotate nets in schematic and PCB.
5. Define final 6- or 8-layer stackup with the PCB manufacturer.
6. Run DRC, SI review and mechanical clearance review.
7. Build a non-electrical mechanical coupon before any powered PCB.
