# RevA schematic concept

## Sheet 1: MXM host connector

Components:

- J1: MXM 3.0 Type-B edge-finger symbol/footprint placeholder.
- TP group: all sideband pins broken out.
- DNI strap resistors for presence/detect and EC-facing signals.

Notes:

- Use a mechanically accurate MXM Type-B outline before PCB work.
- Pin numbering must be verified against the chosen connector/edge-footprint convention.
- Do not trust generic internet pinouts for HP-specific behavior.

## Sheet 2: PCIe x4 high-speed path

Nets:

- PET/PER lanes 0-3.
- REFCLK pair.
- Optional AC-coupling capacitor footprints only where required by the interface convention. Do not blindly duplicate endpoint-side coupling if the dock/card already provides it.

Components:

- J2: OCuLink 4i connector.
- Optional low-cap ESD arrays, DNI by default until SI budget is reviewed.
- Optional common-mode choke footprints are not recommended for first spin unless justified by the connector/cable vendor.

Routing:

- No vias if possible between MXM breakout and OCuLink connector.
- If vias are unavoidable, use backdrill-capable stackup or keep stubs minimal.
- Length-match within each differential pair first; inter-lane matching is secondary for PCIe.

## Sheet 3: sideband and strap matrix

Signals:

- PERST#
- CLKREQ#
- WAKE#
- PRSNT#/detect
- SMBus SCL/SDA
- optional THERM#/ALERT#/FAN_TACH-like signals if HP exposes them on MXM

Implementation:

- 0-ohm jumpers for direct pass-through vs isolation.
- Pull-up/down resistor footprints for detect straps.
- Series resistor placeholders on sideband lines.
- Test pads before and after jumpers.

Default population:

- PERST#: pass-through populated.
- PRSNT#/detect: unpopulated until measured.
- SMBus: isolated by default.
- CLKREQ#/WAKE#: testable; populate as required.
- Thermal spoof: DNI.

## Sheet 4: protected low-power domain

Goal:

Only provide local pull-ups or indicators if required. Do not create a significant load on the MXM power rails.

Possible components:

- Resettable fuse or eFuse for any 3.3 V use.
- TVS/ESD for exposed sideband connector pins.
- Power-good LED, DNI by default.

## Sheet 5: mechanical and debug

- Mounting holes matching safe internal locations only after ZBook mechanical survey.
- Strain relief for OCuLink cable.
- Ground test pads.
- Optional board ID EEPROM footprint, DNI.

## Explicitly excluded from RevA

- PCIe switch.
- Redriver/retimer.
- x8/x16 export.
- ATX power controller.
- External PCIe slot.
- Any attempt to source GPU slot power from the notebook.
