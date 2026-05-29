# 2.4 GHz Inset-Fed Microstrip Patch Antenna

A 2.4 GHz inset-fed microstrip patch antenna designed from first principles on FR4 substrate, built as part of a self-directed RF hardware project targeting aerospace and defense applications. Intended as a single-element radiator suitable for integration into a phased array architecture.

## Project Summary

This project takes a 2.4 GHz patch antenna from analytical design through KiCAD layout and OSHPark fabrication, with planned VNA validation. The goal is end-to-end ownership of an antenna design without relying on a simulator as a crutch: every dimension is computed by hand using standard closed-form microstrip patch formulas, then laid out, fabricated, and measured.

**Current status:** Design complete. PCB layout complete in KiCAD. 3 boards ordered through OSHPark. Awaiting board delivery for VNA-based S11 validation and Smith Chart impedance verification.

## Design Goals

- **Target Frequency:** 2.4 GHz (ISM / Wi-Fi band)
- **Substrate:** FR4 (εr ≈ 4.4), 1.6 mm thickness
- **System Impedance:** 50 Ω
- **Feed Type:** Inset microstrip feed
- **Target Performance:**
  - Input Return Loss (S11): less than -10 dB at 2.4 GHz
  - Resonant frequency centered at 2.4 GHz

## Design Calculations

All dimensions computed analytically using closed-form microstrip patch formulas. The full derivation chain is below.

### Step 1: Patch Width (W)

W = c / (2f) × √(2 / (εr + 1))

With c = 3 × 10⁸ m/s, f = 2.4 × 10⁹ Hz, εr = 4.4:

**W = 38.04 mm**

### Step 2: Effective Dielectric Constant (εreff)

εreff accounts for fringing fields traveling partly through air and partly through FR4.

εreff = (εr + 1)/2 + (εr - 1)/2 × (1 + 12h/W)^(-0.5)

**εreff = 4.086**

### Step 3: Length Extension (ΔL)

Fringing fields make the patch electrically longer than its physical length.

ΔL = 0.412h × [(εreff + 0.3)(W/h + 0.264)] / [(εreff - 0.258)(W/h + 0.8)]

**ΔL = 0.731 mm**

### Step 4: Patch Length (L)

L = c / (2f √εreff) - 2ΔL

**L = 29.44 mm**

### Step 5: Inset Feed Depth (y0)

The inset transforms the high edge radiation resistance (~300 Ω) down to 50 Ω.

y0 = (L/π) × arccos(√(Z0 / Rin))

With Z0 = 50 Ω and Rin = 300 Ω (standard approximation):

**y0 = 5.41 mm** (value used in layout)

Note: VNA measurement will determine whether this inset depth yields the target 50 Ω match. If the measured S11 indicates a mismatch, the inset depth will be adjusted in a v2 board.

### Step 6: 50 Ω Feed Line Width

Standard microstrip width for 50 Ω on FR4 1.6 mm substrate:

**Feed line width = 3.0 mm**

### Step 7: Inset Notch Dimensions

The notch is cut into the patch to allow the feed line to enter without shorting the edge.

- **Notch width:** 1.2 mm
- **Notch depth:** 5.41 mm (matches inset feed depth y0)

## Board Specifications

| Parameter | Value |
|---|---|
| Patch Width (W) | 38.04 mm |
| Patch Length (L) | 29.44 mm |
| Inset Feed Depth (y0) | 5.41 mm |
| Notch Width | 1.2 mm |
| Feed Line Width | 3.0 mm |
| Board Size | 60 mm × 60.44 mm |
| Substrate | FR4, 2-layer, 1.6 mm thick |
| Top Layer | Patch + inset feed line |
| Bottom Layer | Full ground plane |
| Feed Connector | Edge-mount SMA |

## Tools Used

- **KiCAD 9.0** — PCB layout and design files
- **OSHPark** — PCB fabrication (3 copies ordered)
- **VNA** — S11 and impedance validation (planned post-delivery)
- **ANSYS HFSS** — 3D EM simulation comparison (planned)

## Fabrication

- **Fabricator:** OSHPark
- **Quantity:** 3 copies
- **Cost:** $28.10
- **DigiKey BOM:** Samtec SAM8857-ND SMA edge-mount connectors (qty 5)

## Validation Plan (Post-Delivery)

1. Solder edge-mount SMA connector to the inset feed line
2. Connect board to VNA, calibrate at the SMA reference plane
3. Sweep from approximately 1.5 GHz to 3.5 GHz to capture the full resonance shape
4. Measure S11:
   - Confirm resonance is centered near 2.4 GHz
   - Confirm dip depth is below -10 dB
   - Capture impedance trajectory on Smith Chart for inset depth verification
5. Save Touchstone (.s1p) trace and screenshots to the docs/ folder in this repository
6. If accessible, take radiation pattern measurement in an anechoic chamber or open-field setup
7. Compare measured S11 against analytical prediction; document any discrepancies and root-cause them
8. If a v2 board is needed, update y0 / W / L / notch dimensions accordingly and re-fabricate

## Project Status

- [x] Patch dimensions calculated from first principles
- [x] KiCAD schematic and PCB layout complete
- [x] 3 boards ordered from OSHPark
- [ ] Boards delivered
- [ ] SMA connector soldered
- [ ] VNA-based S11 measurement
- [ ] Smith Chart impedance verification
- [ ] ANSYS HFSS 3D simulation comparison
- [ ] Measured vs analytical comparison documented
- [ ] (If needed) v2 board with adjusted inset depth

## References

- *Antenna Theory: Analysis and Design* — Constantine A. Balanis
- *Microwave Engineering* — David M. Pozar
- [Antenna-Theory.com patch antenna design guide](https://antenna-theory.com/antennas/patches/antenna.php)

## License

Hardware design files (schematics, PCB layouts, BOM, Gerbers) are released under the **CERN Open Hardware License Version 2 - Permissive (CERN-OHL-P)**. Software and documentation are released under the **MIT License**.

---

*Project in active development. Boards not yet delivered. Measurements not yet taken. README will be updated as milestones complete.*
