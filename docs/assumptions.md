# Design assumptions

## Target platform

- Host notebook: HP ZBook 17 G5 mobile workstation.
- CPU family: Intel 8th Gen H-series, user-reported i7-8850H.
- Internal dGPU slot: MXM 3.0 Type B class connector/module form factor.
- Existing external path: M.2 NVMe to OCuLink is treated as the lower-risk baseline.

## Known unknowns

The following must be verified by measurement or official board-level documentation before any PCB is fabricated:

1. Exact HP MXM pin usage and any vendor-specific straps.
2. Whether the ZBook firmware enumerates a non-GPU PCIe endpoint on the MXM link.
3. Required PRSNT#/module-detect behavior.
4. Required thermal/fan/EC signals when no HP MXM module is present.
5. Whether PEG lanes are x16-only or can train down to x8/x4 without a valid MXM vBIOS device.
6. Whether the system boots without an installed MXM graphics module.
7. Whether internal display routing depends on the removed MXM dGPU.

## Conservative electrical assumptions

- Use PCIe Gen3 as the design target.
- Assume 85 ohm differential impedance for PCIe data pairs.
- Assume 100 ohm differential impedance for REFCLK unless connector/cable vendor specifies otherwise.
- Treat all high-speed lanes as polarity-sensitive until verified.
- Avoid passive cable lengths beyond validated OCuLink cable assemblies.
- Do not route desktop slot power through the notebook-side PCB.

## Firmware assumptions

The HP BIOS/EC may require MXM presence and thermal-management signals even if the external GPU is electrically valid. RevA must therefore expose test pads and strap options for:

- PRSNT#/module detect
- SMBus pull-ups/isolation
- WAKE#
- CLKREQ#
- PERST# observation/injection
- optional thermal sensor spoofing footprint, DNI by default

## Design philosophy

RevA is a measurement-first board. It should make the platform observable before it tries to maximize performance.
