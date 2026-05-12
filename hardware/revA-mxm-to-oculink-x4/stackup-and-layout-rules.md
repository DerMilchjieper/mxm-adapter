# Stackup and layout rules

## Recommended PCB stackup

Minimum: 6 layers. Preferred: 8 layers if board area and escape routing are tight.

### 6-layer candidate

1. L1: high-speed signals / short escapes
2. L2: solid GND reference
3. L3: low-speed signals / sideband
4. L4: power islands / sideband
5. L5: solid GND reference
6. L6: high-speed signals / connector escape

### 8-layer candidate

1. L1: high-speed signals
2. L2: solid GND
3. L3: high-speed signals if needed
4. L4: solid GND
5. L5: low-speed / power islands
6. L6: solid GND
7. L7: sideband
8. L8: mechanical/debug/low-speed

## Impedance targets

- PCIe data pairs: 85 ohm differential target.
- REFCLK: follow connector/cable vendor guidance; commonly routed as 100 ohm differential clock.
- Use the PCB fabricator's field solver for the chosen stackup.

## Routing constraints

- Keep MXM-to-OCuLink high-speed path as short as mechanically possible.
- Avoid layer changes for PCIe pairs where possible.
- If vias are required, use symmetric via transitions and minimize stubs.
- Maintain continuous GND reference under every high-speed pair.
- Do not cross plane splits.
- Avoid test pads, stubs and probing structures on high-speed pairs.
- Do not place ESD devices on high-speed nets unless their capacitance and placement are validated.
- Keep pair-to-pair spacing wide enough to control crosstalk.
- Length match P/N within each pair according to fabricator/SI guidance.
- Inter-lane matching is less critical than intra-pair matching for PCIe but should still be sane.

## OCuLink connector area

- Follow the chosen connector vendor's footprint exactly.
- Provide shell/shield grounding pads and stitching vias.
- Add mechanical reinforcement if the cable exits the chassis with stress.
- Define whether shield bonds directly to digital GND or via RC/ESD structure after mechanical/chassis review.

## MXM edge area

- Edge fingers require correct thickness, bevel and plating.
- Confirm insertion depth and keeper/retention behavior in the ZBook MXM socket.
- Verify no components collide with heatsink, frame, keyboard deck, fan duct or bottom cover.

## Redriver/retimer decision

RevA starts passive. Add redriver/retimer only after:

1. Passive x4 fails to train reliably at Gen3.
2. Gen2 works but Gen3 is unstable.
3. Cable length cannot be reduced.
4. Eye/SI evidence suggests insertion loss is the dominant issue.

A retimer spin is a different architecture and should become RevB.
