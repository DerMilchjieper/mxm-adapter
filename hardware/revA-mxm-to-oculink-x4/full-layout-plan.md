# Full RevA layout plan

## Objective

Create a measurement-first MXM Type-B to OCuLink x4 interposer that can be mechanically tested in the HP ZBook 17 G5 and electrically validated against a powered external OCuLink eGPU dock.

## Board concept

- Approximate board class: MXM-B-sized interposer / edge-card surrogate.
- Provisional outline: 94 mm x 32 mm.
- Edge: MXM placeholder at notebook socket side.
- Cable: OCuLink 4i connector near outer edge/cable-exit side.
- Debug: sideband strap matrix and test pads placed away from high-speed corridor.

## Functional blocks

### J1: MXM Type-B host edge

Purpose:

- Mate with the ZBook MXM socket.
- Pick off PCIe lane group 0-3, REFCLK and sideband pins.

Status:

- Placeholder only.
- Requires verified edge-card geometry and HP-specific pin map.

### J2: OCuLink 4i connector

Purpose:

- Export four PCIe lanes to a powered eGPU dock.

Status:

- Placeholder only.
- Replace with exact vendor part footprint before PCB release.

### JP1: sideband strap matrix

Purpose:

- Allow controlled population of PERST#, CLKREQ#, WAKE#, SMBus, PRSNT/detect and possible EC straps.

Default population:

- PERST#: 0R pass-through after verification.
- CLKREQ#: DNI initially; populate if needed.
- WAKE#: DNI initially; populate if needed.
- SMBus: DNI/isolate by default.
- PRSNT/detect: DNI until measured.

### TPx: debug pads

Purpose:

- Probe PERST#, CLKREQ#, WAKE#, SMBus, PRSNT and low-power rails.

Rule:

- No test pads on high-speed PCIe pairs in RevA.

## Routing strategy

1. Lock mechanical outline and connector positions first.
2. Route PCIe lane 0-3 pairs from MXM to OCuLink with the shortest possible path.
3. Route REFCLK as a differential clock pair with continuous reference plane.
4. Add ground stitching around connector transitions.
5. Route sideband signals around the high-speed corridor.
6. Add no high-speed stubs, no unnecessary vias and no high-speed test pads.
7. Add silkscreen warnings and revision labels.

## Suggested layer usage

### 6-layer RevA

| Layer | Use |
|---|---|
| L1 | PCIe/REFCLK primary routing |
| L2 | solid GND reference |
| L3 | sideband / low-speed |
| L4 | low-power islands / sideband |
| L5 | solid GND reference |
| L6 | optional high-speed escape / sideband |

## Differential-pair rules

- PCIe data: target 85 ohm differential.
- REFCLK: target per connector/vendor guidance, commonly 100 ohm differential.
- Final width/gap must come from fabricator stackup calculator, not the placeholder KiCad values.
- Match P/N within each pair.
- Keep inter-pair spacing generous.
- Avoid plane splits.
- Avoid via stubs.

## Placement constraints

### MXM edge

- No tall components near insertion edge.
- No copper/parts in mechanical keepout regions until socket geometry is verified.
- Edge bevel/plating must be manufacturer-reviewed.

### OCuLink connector

- Place near cable exit.
- Add mechanical strain relief to chassis or bracket, not the motherboard.
- Do not let cable insertion force lever against MXM socket.

### Strap/debug area

- Place on accessible top side if the bottom cover is removed.
- Keep away from high-speed channel.
- Label all straps clearly.

## Proposed schematic sheets

1. `J1_MXM_Host`: MXM connector, HP pin verification notes.
2. `PCIE_x4_Path`: lane 0-3 and REFCLK pass-through.
3. `J2_OCuLink`: OCuLink connector and shell/shield strategy.
4. `Sideband_Straps`: PERST, CLKREQ, WAKE, SMBus, PRSNT, optional thermal.
5. `Debug_Protection`: low-power fuse, test pads, ESD footprints DNI.
6. `Mechanical`: mounting/keepout notes and fabrication warnings.

## Fabrication gate

Do not generate Gerbers until all of the following are true:

- MXM pin mapping verified.
- MXM edge geometry verified with coupon.
- OCuLink footprint replaced with real vendor drawing.
- Stackup impedance solved with fabricator.
- DRC passes.
- SI review completed.
- Power isolation reviewed.
- ZBook boot behavior with missing MXM module understood.

## RevA population variants

### Variant A: passive-minimal

- J1, J2 populated.
- PERST# pass-through only after verified.
- All SMBus/detect/thermal straps DNI.

### Variant B: measured-detect

- Same as A.
- PRSNT/detect straps populated to match measured original HP MXM module behavior.

### Variant C: EC-compatible

- Same as B.
- Optional thermal/EC spoof components populated if HP EC requires them.

## Why no x16 in this layout

A full x16 riser from MXM would require a different connector/cable/backplane strategy, likely redrivers/retimers, and far more severe signal-integrity review. It also increases mechanical risk inside the laptop. RevA x4 is the safe proof-of-concept path.
