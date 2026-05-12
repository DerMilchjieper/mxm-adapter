# Validation plan

## Phase 0: non-invasive platform survey

1. Photograph MXM bay, heatsink, retention hardware and cable exit candidates.
2. Record current internal GPU model, BIOS version and graphics mode settings.
3. Boot with original MXM GPU and collect:
   - Windows Device Manager screenshot
   - GPU-Z bus interface
   - Linux `lspci -vv` if available
   - BIOS graphics settings
4. Identify whether the notebook can boot with MXM module removed.

## Phase 1: electrical measurement before PCB

With power disconnected:

- Map grounds on MXM area.
- Identify low-impedance power pins carefully.
- Verify likely presence/detect pins against known MXM references.

With original MXM GPU installed and system powered:

- Observe PERST# timing.
- Observe REFCLK presence if suitable equipment is available.
- Observe CLKREQ#/WAKE# state.
- Observe SMBus activity only with high-impedance probing.

Do not short or strap unknown pins.

## Phase 2: dummy interposer / continuity article

Fabricate a no-connect mechanical coupon first if possible:

- Validate edge-card thickness and bevel.
- Validate insertion depth.
- Validate clearance under bottom cover/heatsink region.
- Validate OCuLink cable exit without stressing motherboard.

## Phase 3: RevA passive x4 bring-up

1. Install RevA with no external GPU connected.
2. Verify notebook power-on behavior.
3. Connect powered OCuLink dock with low-risk test GPU.
4. Boot with external dock powered first.
5. Check enumeration:
   - Linux: `lspci -nn`, `lspci -vv`
   - Windows: Device Manager, GPU-Z
6. Check negotiated link:
   - width: x1/x2/x4
   - speed: Gen1/Gen2/Gen3
7. Install driver only after stable enumeration.

## Phase 4: stability testing

- Cold boot repeatability: 20 cycles.
- Warm reboot repeatability: 20 cycles.
- GPU idle stability: 1 hour.
- PCIe link retrain monitoring.
- Light 3D workload.
- Heavy 3D workload.
- Sleep/resume is not required for RevA and should be disabled initially.

## Phase 5: failure classification

| Symptom | Likely class | Next action |
|---|---|---|
| Notebook does not power on | detect/power/EC conflict | remove immediately, inspect straps |
| Boots but no PCIe device | reset/clock/presence/lane issue | inspect PERST#/REFCLK/PRSNT |
| Device appears at Gen1 only | SI/cable/insertion loss | shorten path, consider RevB redriver |
| Device appears x1/x2 only | lane mapping/lane training issue | validate lane order and polarity |
| Driver loads then crashes | power/dock/GPU/driver issue | test known-good GPU/dock |
| Random disconnects under load | SI or dock power | log AER errors, reduce Gen speed |

## Success criteria for RevA

- System boots consistently.
- External GPU enumerates.
- Link trains at PCIe Gen3 x4 or stable Gen2 x4.
- No thermal or EC faults.
- No unsafe heating on notebook-side board.
- No dependence on hotplug.
