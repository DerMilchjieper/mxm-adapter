# mxm-adapter

Research and staged hardware concept for reusing the HP ZBook 17 G5 MXM dGPU connector as an external PCIe/OCuLink host link.

## Current design decision

**RevA targets MXM -> OCuLink x4, not MXM -> external PCIe x16.**

Reasoning:

- x4 keeps the first prototype small enough to route and debug.
- OCuLink provides a mechanically realistic cable exit from a laptop chassis.
- Existing OCuLink eGPU docks can supply the PCIe x16 slot and GPU power externally.
- A passive MXM -> external x16 riser has significantly higher signal-integrity and mechanical risk.
- Dual external GPUs are out of scope for RevA and require either host bifurcation that HP has not documented or an active PCIe switch.

## Safety boundary

Do **not** power a desktop PCIe slot or desktop GPU from the notebook MXM rails. The external GPU dock/backplane must have its own ATX/SFX/server PSU. The notebook-side board should only carry PCIe, clock, reset, presence/sideband signals, and protected low-power logic where required.

## Repository layout

```text
.
├── docs/
│   ├── architecture.md
│   ├── assumptions.md
│   ├── dual-gpu-feasibility.md
│   ├── risk-register.md
│   └── validation-plan.md
├── hardware/
│   └── revA-mxm-to-oculink-x4/
│       ├── block-diagram.md
│       ├── netmap.csv
│       ├── schematic-concept.md
│       ├── stackup-and-layout-rules.md
│       └── bom-categories.md
└── README.md
```

## RevA milestone goals

1. Build a passive/mostly-passive MXM Type-B host-side interposer concept.
2. Route PCIe lanes 0-3, REFCLK, PERST#, CLKREQ#, WAKE#, SMBus, PRSNT and selected power-state signals to an OCuLink 4i connector.
3. Keep GPU slot power fully outside the laptop.
4. Validate boot behavior, PCIe enumeration, link width and link speed before attempting x8/x16.

## Non-goals for RevA

- No hotplug support.
- No desktop PCIe slot inside the notebook.
- No x8/x16 cable riser.
- No dual-GPU switch board.
- No attempt to spoof a full HP MXM GPU module unless measurements prove HP EC/BIOS requires it.

## Status

Concept-only. Not fabrication-ready. HP-specific MXM socket behavior must be measured on the actual ZBook 17 G5 before PCB release.
