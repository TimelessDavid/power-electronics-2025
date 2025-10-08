# Module 4: Thyristor-Based Phase Control

This document covers the theoretical and practical aspects of thyristors (SCRs), focusing on their role in phase-controlled rectification for power regulation applications, as exemplified in Rashid's *Power Electronics*.

---

## 1. Introduction to Thyristors (SCRs)

Thyristors, specifically Silicon Controlled Rectifiers (SCRs), are fundamental components in power electronics for controlling and converting AC power. They function as unidirectional, phase-controlled switches, enabling precise adjustment of output voltage and power delivered to a load such as a lamp.

A typical application involves regulating the brightness of a 12 V / 21 W incandescent lamp by modulating the conduction period of an SCR within a single-phase controlled rectifier.

---

## 2. SCR Structure and Operation

### 2.1. Device Structure

An SCR is a four-layer, three-terminal semiconductor device (p-n-p-n), comprising:

- **Anode (A):** Main current input
- **Cathode (K):** Main current output
- **Gate (G):** Control terminal

SCRs remain non-conductive (blocking state) until triggered by a gate pulse, after which they latch into conduction (ON state) as long as current remains above a holding threshold.

<div align="center">
  <img src="images/scr-structure.png" alt="SCR Device Structure" />
  <p><i>Figure 1: Schematic and symbolic representation of the SCR (Silicon Controlled Rectifier).</i></p>
</div>

### 2.2. Principle of Operation

- **Forward Blocking:** SCR blocks current with anode positive unless a gate trigger is applied.
- **Triggering:** A gate current pulse (I<sub>G</sub>) initiates conduction if the anode-cathode voltage (V<sub>AK</sub>) is positive.
- **Conduction:** Once ON, the SCR continues conducting until the anode current (I<sub>A</sub>) drops below the holding current (I<sub>H</sub>), usually at the next zero crossing in AC.
- **Turn-Off:** SCR naturally turns off when the current falls below I<sub>H</sub> ("natural commutation" in AC circuits).

**Key Parameters:**
- **I<sub>GT</sub>:** Gate trigger current
- **V<sub>DRM</sub>:** Peak repetitive off-state voltage
- **I<sub>T(AV)</sub>:** Average on-state current rating

---

## 3. Phase Control with SCRs

### 3.1. Problem Statement

The goal is to modulate the average DC voltage (V<sub>DC</sub>) applied to a lamp by controlling the firing angle (α) of the SCR in a single-phase rectifier. This enables dimming by adjusting the portion of the AC waveform delivered to the load.

- **AC Input:** 12 V<sub>rms</sub> from transformer (120/12 Vac)
- **Controlled Output:** 9–12 V<sub>DC</sub> (variable)
- **Load:** 12 V, 21 W incandescent lamp

### 3.2. Phase Control Principle

The SCR is triggered at a variable angle α (0–180°) during each positive half-cycle. The later the SCR is triggered (higher α), the less average power is delivered to the load.

**Average Output Voltage (half-wave, purely resistive load):**

$$
V_{DC} = \frac{V_{m}}{2\pi} (1 + \cos\alpha)
$$

- **V<sub>m</sub>:** Peak input voltage = V<sub>rms</sub> × √2

**Example:**
- For V<sub>rms</sub> = 12 V, V<sub>m</sub> ≈ 17 V

---

## 4. Triggering Circuits and Components

### 4.1. Firing (Trigger) Circuit

A common method for SCR phase control is the use of an RC-DIAC triggering circuit, which generates a sharp pulse to the SCR gate at a programmable delay (α) after each zero crossing.

- **RC Network:** Sets time delay (phase angle α)
- **DIAC:** Acts as a threshold device, discharging the capacitor rapidly to provide a sharp gate pulse

**Operation:**
1. C charges through R after each zero crossing.
2. When V<sub>C</sub> reaches DIAC breakover voltage (V<sub>BO</sub>), DIAC fires.
3. Sudden discharge through the gate triggers the SCR.

```python
def calc_alpha(R, C, V_BO, V_peak, f):
    """
    Estimates the firing angle (alpha) based on R, C, DIAC breakover voltage,
    input peak voltage, and line frequency.
    """
    import math
    T = 1 / f
    alpha = math.acos(1 - (V_BO / V_peak))
    return math.degrees(alpha)
```

### 4.2. Commercial Components

**SCR Recommendation:**
- **Model:** C106B Series
- **I<sub>T(AV)</sub>:** 4 A (sufficient for 1.75 A lamp current)
- **V<sub>DRM</sub>:** 100–600 V (well above application requirement)

**DIAC Recommendation:**
- **Model:** DB3
- **V<sub>BO</sub>:** 32 V (typical for triggering circuits)

---

## 5. DC Thyristor Switching Circuit

A fundamental application of the SCR (thyristor) is as a DC switch, allowing or interrupting current to a load by means of gate triggering.

### 5.1. Circuit Description

The following circuit demonstrates the use of an SCR as a controlled switch in a DC circuit:

<div align="center">
  <img src="images/DC_Thyristor_Switching_Circuit.png" alt="DC Thyristor Switching Circuit" />
  <p><i>Figure 2: DC Thyristor Switching Circuit.</i></p>
</div>

**Operation:**
- **S1 (ON):** When pressed, applies a gate current (I<sub>G</sub>) through resistors R<sub>G</sub> and R<sub>GK</sub>, triggering the SCR into conduction. The load receives current (I<sub>A</sub>).
- **S2 (OFF):** When pressed, the anode current drops below the holding current, turning off the SCR and disconnecting the load.
- **R<sub>G</sub> and R<sub>GK</sub>:** Limit current through the gate for safe operation.

**Key Points:**
- This demonstrates the latching behavior of SCRs: Once triggered, the device remains ON even if S1 is released, until S2 is pressed or the supply is interrupted.
- The circuit is commonly used in basic power electronics labs to illustrate SCR operation in DC environments.

---

## 6. Half Wave Phase Control

A widely used application of the SCR is in **half-wave phase control**, enabling adjustment of power delivered to an AC load (like a lamp) by varying the firing angle.

<div align="center">
  <img src="images/Half_Wave_Phase_Control.png" alt="Half Wave Phase Control" />
  <p><i>Figure 3: Half Wave Phase Control circuit with SCR, showing conduction intervals for the lamp.</i></p>
</div>

**Operation:**
- The circuit consists of a variable resistor (R<sub>1</sub>) and capacitor (C) to delay the firing of the SCR after each zero-crossing of the AC supply.
- When the voltage across C reaches the breakover voltage of D<sub>1</sub> (can be a DIAC or simple diode), a pulse is sent to the gate of the SCR.
- The SCR then conducts for the remainder of the half-cycle, powering the lamp.
- By adjusting R<sub>1</sub>, the conduction angle (and thus the brightness of the lamp) can be controlled.

**Key Points:**
- This is the basis of most simple light dimmer circuits.
- The waveform shows the conduction period for the lamp coinciding with when the SCR is ON.

---

## 7. References

[1] Rashid, M. H. (2017). *Power Electronics: Devices, Circuits, and Applications* (4th ed.). Pearson.

[2] Mohan, N., Undeland, T. M., & Robbins, W. P. (2003). *Power Electronics: Converters, Applications, and Design* (3rd ed.). Wiley.

[3] "SCR as a DC Switch" and "Half Wave Phase Control", [Electronics Tutorials](https://www.electronics-tutorials.ws/power/thyristor.html).

[4] DB3 DIAC Datasheet – STMicroelectronics.

[5] C106B SCR Datasheet – ON Semiconductor.

---
