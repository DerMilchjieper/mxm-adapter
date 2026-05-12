# BOM categories

## Connector and mechanical

- MXM Type-B edge-card PCB implementation or mating connector strategy, depending on final mechanical approach.
- OCuLink 4i connector, SFF-8612/SFF-8611 family depending on cable/dock.
- Cable-retention bracket or strain-relief hardware.
- Kapton/insulation films for internal chassis clearance.
- Mounting hardware only where it does not stress the ZBook motherboard.

## Passive high-speed path

- AC-coupling capacitor footprints if required by the chosen topology.
- Optional 0-ohm lane-swap/polarity rework footprints only if they do not create stubs.
- Low-cap ESD arrays, DNI by default.

## Sideband and straps

- 0-ohm jumpers for PERST#, CLKREQ#, WAKE#, SMBus, detect lines.
- Pull-up/pull-down resistor arrays for presence and EC straps.
- Series resistors for sideband damping/isolation.
- Test pads for every sideband net.

## Protection and low-power

- Resettable fuse/eFuse for any local 3.3 V usage.
- TVS for low-speed external-facing nets if exposed.
- Optional LED indicators, DNI by default.

## Debug

- Ground test loops.
- PERST#/CLKREQ#/WAKE# test pads.
- SMBus test pads.
- Optional tiny logic-analyzer header if it fits mechanically.

## External dock requirements

The external dock is treated as a separate assembly and must provide:

- PCIe slot connector.
- ATX/SFX/server PSU input.
- PCIe slot 12 V and 3.3 V.
- AUX GPU power cabling.
- Power switch and power-good behavior.
- Physical GPU support.

RevA should be compatible with an existing powered OCuLink eGPU dock instead of implementing this from scratch.
