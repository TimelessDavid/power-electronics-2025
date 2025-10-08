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
  <p><i>Figure 1: Schematic representation of the SCR structure and symbol.</i></p>
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

## 5. Integrated System Topology

### 5.1. Block Diagram

Power flow summary:

1. **AC 120 V (grid)**
2. **Step-Down Transformer (120 V → 12 V AC)**
3. **Phase-Controlled Rectifier (SCR, adjustable α)**
4. **Variable DC Bus (9–12 V)**
5. **Full-Bridge Inverter (PWM, low-frequency)**
6. **Incandescent Lamp 12 V / 21 W**

<div align="center">
  <img src="images/scr-phase-control-block.png" alt="SCR Phase-Controlled Rectifier Block Diagram" />
  <p><i>Figure 2: Block diagram of the phase-controlled power regulation system using SCR.</i></p>
</div>

---

## 6. Waveforms and Dynamic Behavior

The firing angle α directly determines the shape and average value of the rectified output voltage. Increasing α reduces the conduction period, decreasing output voltage and lamp brightness.

<div align="center">
  <img src="images/phase-control-waveform.png" alt="SCR Output Waveform at Different Alpha" />
  <p><i>Figure 3: Output voltage waveforms for various firing angles α.</i></p>
</div>

---

## 7. References

[1] Rashid, M. H. (2017). *Power Electronics: Devices, Circuits, and Applications* (4th ed.). Pearson.

[2] Mohan, N., Undeland, T. M., & Robbins, W. P. (2003). *Power Electronics: Converters, Applications, and Design* (3rd ed.). Wiley.

[3] DB3 DIAC Datasheet – STMicroelectronics.

[4] C106B SCR Datasheet – ON Semiconductor.

---
