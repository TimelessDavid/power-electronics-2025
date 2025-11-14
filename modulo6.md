# Module 6: DC-AC Converters (Inverters)

*Course:* Power Electronics
*Institution:* Universidad Pontificia Bolivariana - Seccional Montería
*Program:* Facultad de Ingenieria Electrónica



## 1. Introduction to DC-AC Inverters

Power inverters are electronic devices that convert Direct Current (DC) into Alternating Current (AC). This conversion is fundamental to modern power systems, enabling DC sources (like batteries, solar panels, or the output of a rectifier) to power AC loads. The most common topology for this conversion is the Single-Phase Full-Bridge Inverter (also known as an H-Bridge). This circuit uses four switches to generate a variable AC voltage at a desired frequency (e.g., 60 Hz). The primary challenge is not just to create an AC signal, but to create a high-quality, low-distortion sinusoidal output. This is achieved by using high-frequency Pulse Width Modulation (PWM) techniques, specifically Sinusoidal PWM (SPWM). This module details the design, analysis, and simulation of a full-bridge inverter intended to power a 120V (RMS), 20W load from a controlled DC source.

## 2. Full-Bridge Inverter Fundamentals

### 2.1. Topology and Operating Principle

The full-bridge inverter consists of four power switches (typically MOSFETs or IGBTs) arranged in an "H" configuration, with the load connected across the center. Antiparallel freewheeling diodes are required for inductive loads, but are typically internal to power MOSFETs.



The operation relies on switching the four devices in diagonal pairs to control the polarity of the voltage applied to the load:

* *Positive Voltage (+Vdc):* Switches S1 and S4 are turned ON. Current flows from the DC source, through S1, through the load, and through S4 to ground.
* *Negative Voltage (-Vdc):* Switches S2 and S3 are turned ON. Current flows from the DC source, through S3, through the load (in the reverse direction), and through S2 to ground.
* *Zero Voltage (OV):* Achieved by turning ON S1 and S3 (or S2 and S4). This is often used in unipolar switching schemes.

### 2.2. Critical Design Challenge: Shoot-Through and Dead Time

A critical failure mode in an H-Bridge is shoot-through. This occurs if switches in the same "leg" (e.g., S1 and S2) are turned ON simultaneously, creating a direct short circuit across the DC supply. To prevent this, a small delay, known as dead time, must be inserted between turning one switch OFF (e.g., S1) and turning the other switch ON (e.g., S2). This ensures the first switch has fully turned off before the second begins to conduct.

### 2.3. Power Source Integration (Bonus)

As specified in the project criteria, the DC input voltage (Vde) for this inverter should be sourced from a controlled laboratory supply.

* *Bonus Requirement:* This inverter can be powered by the rectifier circuits from Module 1 or Module 4.
* *Uncontrolled (+5%):* Using the three-phase bridge rectifier (Module 1) on a 120V AC (line-to-line) supply would yield a Vdc ≈ 1.35 x 120V = 162V (unregulated).
* *Controlled (+10%):* Using the thyristor-based controlled rectifier (Module 4) allows for a variable DC bus, but a stable 162-170V DC is required for this design.

For this report, we will assume a stable Vdc = 170V input, which is the peak voltage required to generate 120V RMS.

## 3. Modulation and Analysis: Sinusoidal PWM (SPWM)

To create a 60 Hz sine wave, we cannot simply switch at 60 Hz (this would create a square wave). We must use Sinusoidal Pulse Width Modulation (SPWM).

*Principle:* A high-frequency triangular (carrier) wave (f_c, e.g., 20 kHz) is compared against a low-frequency 60 Hz sine (modulating) wave (f_m).

*Generation:*
* When V_sine > V_triangle, the output is switched to +Vde
* When V_sine < V_triangle, the output is switched to -Vdc

### 3.1. Mathematical Analysis

The key parameters are the modulation indices:

* *Amplitude Modulation Index (M_a):* Ratio of sine wave peak (A_m) to triangle wave peak (A_c),
    > M_a = A_m / A_c

The peak amplitude of the fundamental 60 Hz component at the output (V_out,peak) is:
> V_out,peak = M_a x Vde

### 3.2. Design Calculations

1.  *System Requirements:*
    * Load: P = 20 W, V_out,rms = 120 V
    * Output Frequency: f_mi = 60Hz
    * Load Resistance: R = V^2 / P = (120 V)^2 / 20 W = 720 ohms
2.  *Voltage Requirements:*
    * Peak Output Voltage: V_out,peak = V_out,rms x 2 = 120 V x 2 ≈ 170 V
    * Required DC Bus (Vde): To achieve 170V peak with M_a <= 1, we need Vdc >= 170V
    * We will set Vdc = 170 V and use M_a = 1.0
3.  *Current Requirements:*
    * Peak Load Current: I_peak = V_out,peak / R = 170 V / 720 ohms ≈ 0.236 A

## 4. Component Selection and Gate Drive

### 4.1. Power Switches (MOSFETS)

* *Voltage Rating (V_DSS):* Must be greater than Vdc with a safety margin. V_DSS > 1.25 x 170 V = 212.5 V.
* *Selection:* A 250V or 400V-rated MOSFET (e.g., IRF740) is suitable.
* *Current Rating (I_D):* Must be greater than I_peak. The IRF740 (10A) is more than sufficient.

### 4.2. Gate Driver (High-Side Challenge)

This is the most complex part of the design. The "high-side" switches (S1, S3) are floating-their source terminals are not connected to ground. We cannot drive them with a simple ground-referenced signal.

*Solution:* A dedicated half-bridge gate driver IC, such as the IR2110 or IR2104.
*Principle (Bootstrapping):* These ICs use a "bootstrap" capacitor and diode. When the low-side switch (S2) is ON, the capacitor charges from the 12V logic supply. When S2 turns OFF, this charged capacitor "floats" and acts as the local 12V supply for the high-side switch (S1), providing the V_GS > V_S required to turn it on.

### 4.3. Output LC Filter

The SPWM output is a high-frequency (20 kHz) wave with an average 60 Hz sine. We need a low-pass LC filter to remove the switching harmonics and recover the 60 Hz fundamental.

*Design:* The cut-off frequency (f_c) must be between the modulating (60 Hz) and carrier (20 kHz) frequencies. f_c = 1 / (2 * pi * L * C)

*Rule of Thumb:* 10 x f_m < f_c < 0.5 x f_c
Let's choose f_c ≈ 2 kHz.

*Choosing C = 1uF:*
> L = 1 / ((2 * pi * 2000)^2 * 1uF) ≈ 6.3 mH

*Selected Filter:* L = 6.8 mH, C = 1uF

## 5. Simulation and Implementation Results

(This section is to be completed by the student with their LTspice/Multisim data and photos of the hardware implementation.)

### 5.1. Simulation Results

* *SPWM Generator:* (Include image of your SPWM control signal).
* *Unfiltered Output:* (Include oscilloscope plot of the high-frequency SPWM output across the load).
* *Filtered Output:* (Include oscilloscope plot showing the final 60 Hz, 120V RMS sine wave across the 720 ohm load).
* *Expected Waveforms:*

### 5.2. Experimental / Implementation Results

* *Hardware Setup:* (Include photo of the assembled circuit, clearly showing the H-Bridge, IR2110 drivers, and LC filter).
* *Measurements:*
    * DC Bus: Vde = (measured value) V
    * Output: V_out,rms = (measured value) V
    * Frequency: f_m = (measured value) Hz
    * Waveform: (Include oscilloscope capture from the physical circuit).

### 5.3. Analysis and Comparison

(Student to write a paragraph comparing Theory vs. Simulation vs. Experiment. Discuss sources of error, voltage drops, and efficiency.)

## 6. Conclusions

This module successfully demonstrated the design and operation of a single-phase full-bridge inverter. The conversion from a DC bus to a 120V RMS, 60 Hz sinusoidal AC signal was achieved using an SPWM control strategy.

Key learnings from this module include:
* The fundamental H-Bridge topology and its switching states.
* The critical importance of dead time to prevent shoot-through.
* The challenge of high-side gate driving and its solution using bootstrap driver ICs (IR2110).
* The necessity of an LC low-pass filter to extract the fundamental 60 Hz sine wave from the high-frequency PWM output.
* The successful implementation of this inverter completes the full power conversion chain (AC-DC-AC), integrating the principles from all previous modules.

## 7. References

[1] Rashid, M. H. (2017). Power Electronics: Devices, Circuits, and Applications (4th ed.). Pearson.
[2]
