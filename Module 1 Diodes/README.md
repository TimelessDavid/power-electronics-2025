# Module 1: Power Diodes and Rectifiers

## 1. Introduction

Power diodes are semiconductor devices designed for high current and voltage applications in power electronic circuits. This report examines diode characteristics, types, and rectification applications based on established literature [1][2][3].

Power electronics plays a fundamental role in modern electrical energy conversion systems. Among its core components, power diodes and rectifier circuits are essential for converting alternating current (AC) into direct current (DC) in both industrial and consumer applications. Power diodes are designed to handle high current and voltage levels, making them suitable for demanding environments such as motor control systems, power supplies, and renewable energy interfaces.

In particular, single-phase and three-phase rectifiers, composed of power diodes, allow efficient and reliable DC power delivery to various loads. The performance and behavior of these circuits depend greatly on the diode type—general-purpose, fast-recovery, or Schottky—and on how the system is designed to manage ripple, switching losses, and protection.

This report explores the electrical characteristics and functional roles of power diodes, their types, the concept of freewheeling operation, and the analysis and design of rectifier circuits. It also includes an overview of performance parameters, the behavior of RLC circuits in the presence of diodes, and final design considerations.

---

## 2. Power Diode Characteristics

Power diodes operate according to the Shockley equation [1]:

$$I_D = I_S \left( e^{\frac{V_D}{n V_T}} - 1 \right)$$

- **Forward-biased conduction:** Allows current when the anode is more positive than the cathode.
- **Reverse-blocking capability:** Withstands high reverse voltages up to breakdown (VRRM).
- **Reverse recovery time (trr):** Time required to stop conduction after polarity reversal.
- **Forward current capacity (IF):** Typically designed for >1 A continuous current.
- **Forward voltage drop (VF):** Between 0.7 V and 1.2 V, impacts conduction losses.
- **Thermal dissipation:** Requires heat sinks to manage power loss safely.

**Critical Parameters:**
- Maximum reverse voltage (V_RRM): 50V to 6kV
- Average forward current (I_F(AV)): 1A to several kA
- Reverse recovery time (t_rr): 25ns to 30μs
- Forward voltage drop (V_F): 0.7V to 1.5V

<!-- Image placeholder -->
<div align="center">
  <img src="./images/power_diode_structure.png" alt="Power Diode Structure" width="500"/>
  <p><i>Figure 1: Power diode structure</i></p>
</div>

---

## 3. Types of Diodes

**General-Purpose Diodes [1]:**
These diodes are used in low-frequency applications (50/60 Hz).  
- **Reverse recovery time:** Long (25–50 μs).  
- **Applications:** AC line rectifiers, battery chargers.  
- Not suitable for high-speed switching.

**Fast-Recovery Diodes [2]:**
Designed for medium to high-frequency circuits.  
- **Reverse recovery time:** Shorter (1–5 μs).  
- **Applications:** Inverters, DC–DC converters, and snubber circuits.  
- Reduces switching losses and EMI.

**Schottky Diodes [3]:**
Use a metal-semiconductor junction instead of a P–N junction.  
- **Forward voltage drop:** Low (0.3–0.5 V).  
- **Switching speed:** Very fast (nanoseconds).  
- **Applications:** High-speed, low-voltage circuits.  
- Limited reverse voltage (< 100 V) and higher leakage current.

<!-- Image placeholder -->
<div align="center">
  <img src="./images/diode_types_comparison.png" alt="Diode Types" width="500"/>
  <p><i>Figure 2: Diode types comparison</i></p>
</div>

---

## 4. Freewheeling Operation

Freewheeling diodes provide current continuity in inductive circuits when switches open, preventing voltage spikes [1].

**Operation Principle:**
- Inductor energy: $E_L = \frac{1}{2}LI^2$
- Current decay: $i_L(t) = I_0 e^{-\frac{R}{L}t}$
- Voltage limited to diode drop (~1V)

  In power electronics circuits with inductive loads (such as motors or coils), a freewheeling diode is connected in parallel with the load to provide a path for the inductive current when the main switching device (e.g., a transistor or thyristor) turns off.
[3]
- **Protect the circuit** from high-voltage transients caused by the sudden interruption of current through inductance.
- **Maintain current continuity** in the inductive load during the OFF period of the switch.
- **Reduce voltage spikes** across switching devices, enhancing reliability.

This operation is critical in DC chopper circuits, inverter-fed motors, and controlled rectifiers where inductive elements are present.

<!-- Image placeholder -->
<div align="center">
  <img src="./images/freewheeling_circuit.png" alt="Freewheeling Circuit" width="400"/>
  <p><i>Figure 3: Freewheeling diode operation</i></p>
</div>

---

## 5. Rectifiers with Diodes

### 5.1. Single-Phase Full-Wave Bridge

Four-diode bridge configuration for AC to DC conversion [2]:

**Key Parameters:**
- Average voltage: $V_{dc} = \frac{2V_m}{\pi} = 0.637 V_m$
- Ripple factor: $RF = 0.482$
- PIV per diode: $PIV = V_m$

**Simulation Results:**
The single-phase rectifier simulation demonstrates typical full-wave rectification:
- `rectificador_monofasico.png` shows the complete four-diode bridge circuit producing 120V DC output
- Output exhibits characteristic pulsating DC waveform with 100Hz ripple frequency

### 5.2. Three-Phase Full-Wave Bridge

Six-diode configuration with superior performance [3]:

**Key Parameters:**
- Average voltage: $V_{dc} = \frac{3\sqrt{6}V_{LL}}{\pi} = 1.35 V_{LL}$
- Ripple factor: $RF = 0.042$
- PIV per diode: $PIV = \sqrt{3}V_{LL}$

**Simulation Analysis:**
Three-phase rectifier simulations confirm superior performance:
- `rectificador_trifasico_sources.png`: Balanced 120V three-phase sources with 120° displacement
- `rectificador_trifásico.png`: Complete six-diode bridge configuration
- `rectificador_trifasico_V_R2_without_condensor.png`: Six-pulse output with 4.2% ripple
- `rectificador_trifasico_V_R2_with_condensor.png`: Filtered output achieving near-constant 120V DC

<!-- Image placeholder -->
<div align="center">
  <img src="./images/rectificador_trifásico.png" alt="Three-Phase Rectifier Circuit" width="600"/>
  <p><i>Figure 4: Three-phase bridge rectifier configuration</i></p>
</div>

---

## 6. Performance Parameters

**Key Metrics [1][2]:**
- Efficiency: $\eta = \frac{P_{dc}}{P_{ac}}$
- Transformer Utilization Factor: $TUF = \frac{P_{dc}}{VA_{rating}}$
- Total Harmonic Distortion: $THD = \frac{\sqrt{\sum_{n=2}^{\infty} I_n^2}}{I_1}$

**Comparison:**
| Parameter | Single-Phase | Three-Phase |
|-----------|--------------|-------------|
| Ripple Factor | 0.482 | 0.042 |
| Efficiency | ~81% | ~95% |
| TUF | 0.812 | 0.955 |
**Average Output Voltage (Vdc)**
- Represents the DC level of the rectified output.
- For a single-phase full-wave rectifier:  
  Vdc = (2 × Vm) / π

**RMS Output Voltage (Vrms)**
- Useful for calculating power delivered to the load.

**Ripple Factor (r)**
- Indicates the amount of AC variation in the DC output.
- Defined as:  
  r = Vr(ac,rms) / Vdc  
- Lower ripple implies better DC quality.
- 
**Rectification Efficiency (η)**
- Ratio of DC power delivered to the load versus AC power from the source.
- Expressed as:  
  η = (Pdc / Pac) × 100%
**Form Factor (FF)**
- Ratio of Vrms to Vdc:  
  FF = Vrms / Vdc  
- Indicates waveform distortion




---

## 7. Rectifier Circuit Design

**Design Steps [1][3]:**
1. Determine load requirements (V_dc, I_dc)
2. Select topology (single-phase vs three-phase)
3. Choose diodes with safety margins
4. Design filter components

**Component Selection:**
- Diode voltage rating: $V_{RRM} \geq 2 \times PIV$
- Diode current rating: $I_{F(AV)} \geq 1.5 \times I_{load}$
- Filter capacitor: $C = \frac{I_{load}}{2fV_{ripple}}$

**Design Example:**
Output: 24V DC, 5A; Input: 230V AC
- PIV = 37.7V → Select 100V diodes
- Average current = 5A → Select 10A diodes
- Filter: C = 22mF for 5% ripple

<!-- Image placeholder -->
<div align="center">
  <img src="./images/rectifier_design.png" alt="Rectifier Design" width="500"/>
  <p><i>Figure 5: Rectifier design methodology</i></p>
</div>

---

## 8. RLC Circuit Behavior with Diodes

RLC circuits with diodes exhibit nonlinear behavior due to diode switching [1]:

**Diode Clipping Applications:**
Real-world implementations demonstrate diode behavior in signal processing:
- `shunt_clipper.png`: Shunt clipper circuit configuration for peak limiting
- `Shunt_clipper_graphic_possitive_peak_clipping.png`: Positive peak clipping at 10V level, demonstrating diode forward conduction
- `Shunt_clipper_graphic_negative_peak_clipping.png`: Negative peak clipping protection, showing diode reverse blocking

**Signal Slicing:**
- `slicer.png`: Slicer circuit implementing dual-level voltage limiting
- `slicer_graphic.png`: Output waveform showing precise voltage level control between upper and lower clipping levels

**Operating Modes:**
- **Diode ON:** Standard RLC response with $\omega_n = \frac{1}{\sqrt{LC}}$
- **Diode OFF:** Current = 0, capacitor voltage constant

**Applications:** Resonant converters, energy transfer circuits, signal conditioning

<!-- Image placeholder -->
<div align="center">
  <img src="./images/shunt_clipper.png" alt="Shunt Clipper Circuit" width="500"/>
  <p><i>Figure 6: Shunt clipper circuit for peak voltage limiting</i></p>
</div>

---

## 9. Simulations

**Circuit Implementations and Results:**

**Single-Phase Rectifiers:**
- `rectificador_monofasico.png`: Full-wave bridge producing 120V DC with characteristic 48.2% ripple
- `rectificador_monofasico_media_onda.png`: Half-wave rectifier demonstration showing 100% ripple factor
- `rectificador_monofasico_media_onda_condensor.png`: Capacitive filtering reducing ripple to acceptable levels

**Three-Phase Rectifiers:**
Simulation results validate theoretical performance:
- Input: 120V line-to-line RMS, 50Hz three-phase supply
- Output: 120V DC with minimal 4.2% ripple factor
- Filter effectiveness demonstrated in capacitor-filtered configurations

**Signal Processing Applications:**
Clipping and slicing circuits demonstrate practical diode applications in signal conditioning, with precise voltage level control and protection functions.

**File Organization:**
- **[Simulation Files](./simulation_files/):** LTSpice files (.asc)
- **[Images](./images/):** Waveforms and circuit analysis plots

---

## 10. Conclusions

Power diodes play a fundamental role in converting AC to DC in power electronic systems. Their selection—whether general-purpose, fast-recovery, or Schottky—depends on the operating frequency, voltage, and current requirements of the application. Freewheeling diodes are especially useful when working with inductive loads, as they help prevent voltage spikes and maintain current continuity when the main switch is turned off. Rectifiers built with diodes, either single-phase or three-phase, are widely used in industrial and electronic applications, and their performance is evaluated through parameters like average output voltage, ripple, and efficiency. A proper rectifier design considers diode ratings, filtering components, and protection elements such as heat sinks and fuses. Understanding the behavior of RLC loads in these circuits also helps improve stability and overall system performance.

Power diodes are essential components in power electronics with distinct characteristics for different applications:
- **Diode Selection:** General-purpose for low-frequency, fast-recovery for switching, Schottky for high-efficiency applications
- **Rectifier Performance:** Three-phase systems offer superior performance (4.2% vs 48.2% ripple)
- **Design Considerations:** Safety margins and thermal management are critical
- **Future Trends:** SiC and GaN technologies enable higher frequency and efficiency

---

## References

[1] Erickson, R. W., & Maksimović, D. (2001). *Fundamentals of Power Electronics* (2nd ed.). Springer.

[2] Mohan, N., Undeland, T. M., & Robbins, W. P. (2003). *Power Electronics: Converters, Applications, and Design* (3rd ed.). Wiley.

[3] Rashid, M. H. (2017). *Power Electronics: Devices, Circuits, and Applications* (4th ed.). Pearson.
