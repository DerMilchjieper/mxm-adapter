# Risk register

| ID | Risk | Severity | Likelihood | Mitigation |
|---|---|---:|---:|---|
| R1 | Wrong MXM pin mapping damages motherboard | Critical | Medium | Do not fabricate before pinout verification; use continuity/mechanical coupon first |
| R2 | HP BIOS/EC refuses boot without HP MXM GPU | High | Medium | Test boot without MXM; expose detect/thermal straps; keep original module recoverable |
| R3 | PCIe Gen3 signal integrity failure | High | Medium | Start x4 passive; shortest path; controlled impedance; fall back to Gen2; RevB redriver if needed |
| R4 | OCuLink cable mechanical stress damages MXM socket | High | Medium | Add strain relief; avoid rigid lever arm; validate cable exit with dummy board |
| R5 | External GPU power backfeeds notebook | Critical | Low/Medium | Do not connect external slot power to MXM rails; isolate low-power sideband where needed |
| R6 | SMBus conflict between dock/GPU and HP EC | Medium | Medium | SMBus isolated by default; populate jumpers only after measurement |
| R7 | Thermal/fan EC fault due to missing MXM module | Medium | Medium | Identify thermal pins; optional spoof footprint; BIOS testing with original module removed |
| R8 | Hotplug event causes crash or damage | High | Medium | RevA forbids hotplug; require cold connection and dock powered before boot |
| R9 | Internal display no longer works as expected | Medium | Medium | Use external monitor on eGPU; keep iGPU/dGPU BIOS settings documented |
| R10 | Dual GPU setup unstable | Medium | High | Not RevA; validate each path independently before simultaneous testing |
| R11 | Fabricator cannot meet edge-card/mechanical tolerances | High | Medium | Use vendor DFM review; create mechanical coupon first |
| R12 | Legal/licensing issue with proprietary MXM pinout documents | Medium | Low | Do not commit confidential docs; document derived measurements and public references only |

## Go / no-go gates

### Gate A: before schematic capture

- HP-specific MXM behavior has at least a measurement plan.
- Original MXM module can be reinstalled safely.
- Mechanical cable path is plausible.

### Gate B: before PCB fabrication

- Edge-card geometry verified.
- Pin mapping reviewed.
- Power isolation reviewed.
- No unknown power pin is routed to external connector.
- DFM check passed.

### Gate C: before powered test

- Continuity checked.
- Shorts to power and ground checked.
- External dock tested independently.
- Current-limited first power-on plan documented.

### Gate D: before expensive GPU test

- Use sacrificial/low-value GPU first.
- Confirm enumeration and link training.
- Confirm no abnormal heating.
