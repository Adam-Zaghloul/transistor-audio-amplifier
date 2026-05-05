# 3-Stage Discrete Transistor Audio Amplifier

> Common-emitter preamp → emitter-follower buffer → power output stage · 44.3 dB gain · 20 Hz – 20 kHz  
> **La Cité collégiale — Transistors (TEC27910) · Fall 2025**

---

## Overview

A fully discrete 3-stage audio amplifier designed to drive an **8 Ω speaker** from a 3.5 mm audio source (smartphone or computer). Every stage was analyzed with **DC bias-point and AC small-signal calculations** before assembly, then validated on the bench with an oscilloscope.

The design achieves **~44.3 dB of voltage gain** across the full audio bandwidth (20 Hz – 20 kHz), meeting and exceeding the 40 dB target from the design brief.

---

## Specifications

| Parameter | Value |
|-----------|-------|
| Input source | 3.5 mm audio jack (smartphone / PC) |
| Load | 8 Ω speaker |
| Power supply | 9 V DC |
| Voltage gain | ~163.6× (~44.3 dB) |
| Bandwidth | 20 Hz – 20 kHz (full audio) |
| THD | < 5% at nominal power |
| Low-frequency cutoff (−3 dB) | ~20 Hz (set by C2 = 1000 µF + 8 Ω speaker) |

---

## Stage Architecture

```
[3.5 mm Jack] → [C1 1µF coupling] → [Q1 2SC1815 Common-Emitter]
                                              │ gain ≈ 163.6
                                        [Q2 2N3904 Emitter-Follower]
                                              │ impedance buffer
                                        [Q3 BDX53C Common-Collector]
                                              │ high current drive
                                        [C2 1000µF] → [8Ω Speaker]
```

---

## Stage-by-Stage Design

### Stage 1 — Q1 (2SC1815) · Common-Emitter Preamp

The 2SC1815 is a low-noise NPN transistor optimised for audio preamplification. Configured in **common-emitter** for maximum voltage gain.

**DC Bias Calculations:**

| Parameter | Value |
|-----------|-------|
| R1 (upper bias) | 3.3 MΩ |
| R3 (lower bias) | 820 kΩ |
| Vb | 1.79 V |
| Ve | 1.09 V |
| Ie | 109.1 µA |
| Vc | 4.74 V |
| Vce | 3.65 V |
| re (dynamic emitter resistance) | 238.3 Ω |

**AC Gain:**

```
G1 = Rc / re = 39 kΩ / 238.3 Ω ≈ 163.6   →   44.3 dB
```

C3 (47 µF) bypasses R4 at AC frequencies, removing emitter degeneration and maximising gain without affecting the DC bias point.

### Stage 2 — Q2 (2N3904) · Emitter-Follower Buffer

Configured as **collector-common (emitter-follower)**. Provides:
- Voltage gain ≈ 1 (no additional voltage amplification)
- High input impedance — does not load Stage 1
- Lower output impedance — capable of driving Stage 3

### Stage 3 — Q3 (BDX53C) · Power Output Stage

The BDX53C is an NPN Darlington power transistor. Also configured as **collector-common**.
- High current gain — delivers sufficient current to drive the 8 Ω speaker
- Protection diode on base prevents reverse breakdown
- R5 (1 Ω) limits collector current and protects against thermal runaway

C2 (1000 µF) blocks DC from the speaker while allowing audio frequencies to pass. The −3 dB low-frequency cutoff:

```
Fl = 1 / (2π × C2 × R_speaker) = 1 / (2π × 1000µF × 8Ω) ≈ 20 Hz
```

---

## Bill of Materials

| Ref | Component | Value | Function |
|-----|-----------|-------|----------|
| Q1 | 2SC1815 NPN | — | Common-emitter preamp |
| Q2 | 2N3904 NPN | — | Emitter-follower buffer |
| Q3 | BDX53C NPN Darlington | — | Power output stage |
| C1 | Coupling capacitor | 1 µF | Blocks DC from input |
| C2 | Output coupling cap | 1000 µF | Blocks DC from speaker |
| C3 | Emitter bypass cap | 47 µF | Maximises AC gain of Q1 |
| R1 | Bias resistor | 3.3 MΩ | Upper divider for Q1 base |
| R3 | Bias resistor | 820 kΩ | Lower divider for Q1 base |
| R2 | Collector load | 39 kΩ | Sets Stage 1 voltage gain |
| R4 | Emitter resistor | 10 kΩ | DC stability / thermal |
| R5 | Current limit | 1 Ω | Protects Q3 from overcurrent |
| R6 | Speaker load | 8 Ω | Output transducer |
| VCC | Supply | 9 V DC | Powers all stages |

---

## Problems Encountered & Solutions

| Problem | Root Cause | Solution |
|---------|-----------|---------|
| Q3 thermal saturation / overheating | Output stage dissipating too much power | Replaced with BDX53C power Darlington; added R5 current-limit resistor |
| Distortion / crackling at high volume | Input signal too large → Q1 saturation | Reduced input level to keep Q1 in active region |

Each fix was verified with the oscilloscope — waveform captured before and after every change.

---

## Simulation

The full circuit was simulated in **Multisim** before breadboard assembly:
- DC bias verified: Vb = 1.79 V, Vce = 3.66 V ✓
- AC gain measured in simulation: G1 = 164 (44 dB) ✓
- Bandwidth confirmed flat from 20 Hz to 20 kHz ✓
- Distortion < 5% at nominal power ✓

---

## Skills Demonstrated

`Transistor biasing` `AC small-signal analysis` `Common-emitter amplifier` `Emitter-follower` `Darlington power stage` `Multisim simulation` `Oscilloscope validation` `Thermal management` `Audio electronics` `Fault isolation`

---

*Adam Zaghloul · La Cité collégiale · Fall 2025 · [adamzaghloul07@gmail.com](mailto:adamzaghloul07@gmail.com)*
