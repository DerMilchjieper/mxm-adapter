# Roadmap

## Stage 0: baseline OCuLink eGPU

Goal: prove the laptop can use an external GPU through an easier path.

- Use M.2 NVMe -> OCuLink adapter.
- Use powered OCuLink dock.
- Connect monitor directly to GPU.
- Record link speed, link width, boot behavior and driver stability.

Exit criteria:

- Stable boot and gaming/compute workload on M.2 OCuLink.

## Stage 1: MXM platform characterization

Goal: learn how HP handles the MXM slot.

- Document installed MXM module.
- Test boot with MXM module removed if safe.
- Record BIOS behavior.
- Measure sideband timing.
- Identify detect/thermal dependencies.

Exit criteria:

- Known boot behavior with and without MXM module.
- Candidate sideband requirements identified.

## Stage 2: mechanical coupon

Goal: validate board fit without electrical risk.

- Fabricate edge-card outline coupon.
- Validate thickness, bevel, insertion depth and retention.
- Validate OCuLink cable path.
- Validate no chassis/heatsink collision.

Exit criteria:

- Physical fit confirmed.
- Cable strain-relief concept confirmed.

## Stage 3: RevA passive x4 board

Goal: enumerate one external GPU over MXM-derived PCIe x4.

- Route lanes 0-3 to OCuLink.
- Add sideband straps/test pads.
- Keep SMBus isolated by default.
- Use external powered OCuLink dock.

Exit criteria:

- External GPU enumerates.
- Stable Gen2 x4 minimum; Gen3 x4 target.

## Stage 4: RevB robustness spin

Only if RevA proves the platform concept.

Potential additions:

- Redriver/retimer.
- Improved mechanical strain relief.
- Better sideband conditioning.
- Optional x8 if mechanical/SI budget supports it.

## Stage 5: dual-GPU experiment

Only after M.2 OCuLink and MXM OCuLink are independently stable.

- Boot with both external paths populated.
- Record PCIe topology.
- Test AMD gaming GPU + Nvidia compute GPU split.
- Avoid multi-GPU gaming as a target.

## Stage 6: x16 riser exploration

This remains a research branch, not the default product path.

Requirements before attempting:

- Proven MXM enumeration.
- High-speed layout review.
- External PCIe slot power/backplane design.
- Mechanical enclosure concept.
- Acceptance that redriver/retimer may be required.
