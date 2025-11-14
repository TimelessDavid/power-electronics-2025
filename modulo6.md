# Module 6: DC-AC Converters (Inverters)

*Course:* Power Electronics
*Institution:* Universidad Pontificia Bolivariana - Seccional Montería
*Program:* Facultad de Ingenieria Electrónica

[span_0](start_span)

## 1. Introduction to DC-AC Inverters

Power inverters are electronic devices that convert Direct Current (DC) into Alternating Current (AC)[span_0](end_span). [span_1](start_span)This conversion is fundamental to modern power systems, enabling DC sources (like batteries, solar panels, or the output of a rectifier) to power AC loads [1][span_1](end_span). [span_2](start_span)The most common topology for this conversion is the Single-Phase Full-Bridge Inverter (also known as an H-Bridge)[span_2](end_span). [span_3](start_span)This circuit uses four switches to generate a variable AC voltage at a desired frequency (e.g., 60 Hz)[span_3](end_span). [span_4](start_span)The primary challenge is not just to create an AC signal, but to create a high-quality, low-distortion sinusoidal output[span_4](end_span). [span_5](start_span)This is achieved by using high-frequency Pulse Width Modulation (PWM) techniques, specifically Sinusoidal PWM (SPWM)[span_5](end_span). [span_6](start_span)This module details the design, analysis, and simulation of a full-bridge inverter intended to power a 120V (RMS), 20W load from a controlled DC source [2][span_6](end_span).

## 2. Full-Bridge Inverter Fundamentals

### 2.1. Topology and Operating Principle

[span_7](start_span)The full-bridge inverter consists of four power switches (typically MOSFETs or IGBTs) arranged in an "H" configuration, with the load connected across the center[span_7](end_span). [span_8](start_span)Antiparallel freewheeling diodes are required for inductive loads, but are typically internal to power MOSFETs [1][span_8](end_span).

[span_9](start_span)

The operation relies on switching the four devices in diagonal pairs to control the polarity of the voltage applied to the load:

* *Positive Voltage $(+V_{dc})$:* Switches S1 and S4 are turned ON[span_9](end_span). [span_10](start_span)Current flows from the DC source, through S1, through the load, and through S4 to ground[span_10](end_span).
* *[span_11](start_span)Negative Voltage $(-V_{dc})$:* Switches S2 and S3 are turned ON[span_11](end_span). [span_12](start_span)Current flows from the DC source, through S3, through the load (in the reverse direction), and through S2 to ground[span_12](end_span).
* *[span_13](start_span)Zero Voltage (OV):* Achieved by turning ON S1 and S3 (or S2 and S4)[span_13](end_span). [span_14](start_span)This is often used in unipolar switching schemes[span_14](end_span).

### 2.2. Critical Design Challenge: Shoot-Through and Dead Time

[span_15](start_span)A critical failure mode in an H-Bridge is shoot-through[span_15](end_span). [span_16](start_span)This occurs if switches in the same "leg" (e.g., S1 and S2) are turned ON simultaneously, creating a direct short circuit across the DC supply[span_16](end_span). [span_17](start_span)To prevent this, a small delay, known as dead time, must be inserted between turning one switch OFF (e.g., S1) and turning the other switch ON (e.g., S2)[span_17](end_span). [span_18](start_span)This ensures the first switch has fully turned off before the second begins to conduct [3][span_18](end_span).

### 2.3. [span_19](start_span)Power Source Integration (Bonus)[span_19](end_span)

[span_20](start_span)As specified in the project criteria, the DC input voltage $(V_{de})$ for this inverter should be sourced from a controlled laboratory supply[span_20](end_span).

* *[span_21](start_span)Bonus Requirement:* This inverter can be powered by the rectifier circuits from Module 1 or Module 4[span_21](end_span).
* *[span_22](start_span)Uncontrolled (+5%):* Using the three-phase bridge rectifier (Module 1) on a 120V AC (line-to-line) supply would yield a $V_{dc}\approx1.35\times120V=162V$ (unregulated)[span_22](end_span).
* *[span_23](start_span)Controlled (+10%):* Using the thyristor-based controlled rectifier (Module 4) allows for a variable DC bus, but a stable 162-170V DC is required for this design[span_23](end_span).

[span_24](start_span)For this report, we will assume a stable $V_{dc}=170V$ input, which is the peak voltage required to generate 120V RMS[span_24](end_span).

## 3. Modulation and Analysis: Sinusoidal PWM (SPWM)

[span_25](start_span)To create a 60 Hz sine wave, we cannot simply switch at 60 Hz (this would create a square wave)[span_25](end_span). [span_26](start_span)We must use Sinusoidal Pulse Width Modulation (SPWM)[span_26](end_span).

*[span_27](start_span)Principle:* A high-frequency triangular (carrier) wave ( $f_{c}$ e.g., 20 kHz) is compared against a low-frequency 60 Hz sine (modulating) wave $(f_{m})$[span_27](end_span).

*[span_28](start_span)Generation:*[span_28](end_span)
* [span_29](start_span)When $V_{sine}>V_{triangle}$, the output is switched to $+V_{de}$[span_29](end_span)
* [span_30](start_span)When $V_{sine}<V_{triungle}$ the output is switched to $-V_{dc}$[span_30](end_span)

### 3.1. [span_31](start_span)Mathematical Analysis[span_31](end_span)

[span_32](start_span)The key parameters are the modulation indices:[span_32](end_span)

* *[span_33](start_span)Amplitude Modulation Index $(M_{a})$:* Ratio of sine wave peak $(A_{m})$ to triangle wave peak ( $(A_{c}),$[span_33](end_span)
    > [span_34](start_span)$M_{a}=\frac{A_{m}}{A_{c}}$[span_34](end_span)

[span_35](start_span)The peak amplitude of the fundamental 60 Hz component at the output $(V_{out,peak})$ is:[span_35](end_span)
> [span_36](start_span)$V_{oit,peak}=M_{a}\times V_{de}$[span_36](end_span)

### 3.2. Design Calculations

1.  *[span_37](start_span)System Requirements:*[span_37](end_span)
    * [span_38](start_span)Load: $P=20~W$ $V_{out,rms}=120~V$[span_38](end_span)
    * [span_39](start_span)Output Frequency: $f_{mi}=60Hz$[span_39](end_span)
    * [span_40](start_span)Load Resistance: $R=V^{2}/P = (120 V)^{2}/20 W=720\Omega$[span_40](end_span)
2.  *[span_41](start_span)Voltage Requirements:*[span_41](end_span)
    * [span_42](start_span)Peak Output Voltage: $V_{out,peak}=V_{out,rms}\times2=120~V\times2\approx170~V$[span_42](end_span)
    * [span_43](start_span)Required DC Bus $(V_{de})$: To achieve 170V peak with $M_{a}\le1$ we need $V_{dc}\ge170V$[span_43](end_span)
    * [span_44](start_span)We will set $V_{dc}=170~V$ and use $M_{u}=1.0$[span_44](end_span)
3.  *[span_45](start_span)Current Requirements:*[span_45](end_span)
    * [span_46](start_span)Peak Load Current: $I_{peak}=V_{out,peak}/R=170~V/720~\Omega\approx0.236~A$[span_46](end_span)

## 4. Component Selection and Gate Drive

### 4.1. Power Switches (MOSFETS)

* *Voltage Rating $(V_{DSS})$:* Must be greater than $V_{dc}$ with a safety margin. [span_47](start_span)$V_{DSS}>1.25\times170~V=212.5~V.$[span_47](end_span)
* *[span_48](start_span)Selection:* A 250V or 400V-rated MOSFET (e.g., IRF740) is suitable[span_48](end_span).
* *Current Rating $(I_{D})$:* Must be greater than $I_{peak}$. [span_49](start_span)The IRF740 (10A) is more than sufficient[span_49](end_span).

### 4.2. [span_50](start_span)Gate Driver (High-Side Challenge)[span_50](end_span)

[span_51](start_span)This is the most complex part of the design[span_51](end_span). [span_52](start_span)The "high-side" switches (S1, S3) are floating-their source terminals are not connected to ground[span_52](end_span). [span_53](start_span)We cannot drive them with a simple ground-referenced signal[span_53](end_span).

*[span_54](start_span)Solution:* A dedicated half-bridge gate driver IC, such as the IR2110 or IR2104[span_54](end_span).
*[span_55](start_span)Principle (Bootstrapping):* These ICs use a "bootstrap" capacitor and diode[span_55](end_span). [span_56](start_span)When the low-side switch (S2) is ON, the capacitor charges from the 12V logic supply[span_56](end_span). [span_57](start_span)When S2 turns OFF, this charged capacitor "floats" and acts as the local 12V supply for the high-side switch (S1), providing the $V_{GS}>V_{S}$ required to turn it on[span_57](end_span).

### 4.3. [span_58](start_span)Output LC Filter[span_58](end_span)

[span_59](start_span)The SPWM output is a high-frequency (20 kHz) wave with an average 60 Hz sine[span_59](end_span). [span_60](start_span)We need a low-pass LC filter to remove the switching harmonics and recover the 60 Hz fundamental[span_60](end_span).

*[span_61](start_span)Design:* The cut-off frequency $(f_{c})$ must be between the modulating (60 Hz) and carrier (20 kHz) frequencies[span_61](end_span). [span_62](start_span)$f_{c}=\frac{1}{2\pi LC}$[span_62](end_span)

*[span_63](start_span)Rule of Thumb:* $10\times f_{m}<f_{c}<0.5\times f_{c}$[span_63](end_span)
[span_64](start_span)Let's choose $f_{c}\approx2$ kHz[span_64](end_span).

*[span_65](start_span)Choosing $C=1\mu F$:*[span_65](end_span)
> [span_66](start_span)$L=\frac{1}{(2\pi f_{e})^{2}C}=\frac{1}{(2\pi\times2000)^{2}\times1\mu F}\approx6.3~mH$[span_66](end_span)

*[span_67](start_span)Selected Filter:* $L=6.8~mH$ , $C=1\mu F$[span_67](end_span)

## 5. Simulation and Implementation Results

(This section is to be completed by the student with their LTspice/Multisim data and photos of the hardware implementation.)

### 5.1. [span_68](start_span)Simulation Results[span_68](end_span)

* *[span_69](start_span)SPWM Generator:* (Include image of your SPWM control signal)[span_69](end_span).
* *[span_70](start_span)Unfiltered Output:* (Include oscilloscope plot of the high-frequency SPWM output across the load)[span_70](end_span).
* *[span_71](start_span)Filtered Output:* (Include oscilloscope plot showing the final 60 Hz, 120V RMS sine wave across the 720 $\Omega$ load)[span_71](end_span).
* *[span_72](start_span)Expected Waveforms:*[span_72](end_span)

### 5.2. Experimental / Implementation Results

* *[span_73](start_span)Hardware Setup:* (Include photo of the assembled circuit, clearly showing the H-Bridge, IR2110 drivers, and LC filter)[span_73](end_span).
* *[span_74](start_span)Measurements:*[span_74](end_span)
    * [span_75](start_span)DC Bus: $V_{de}=$ (measured value) V[span_75](end_span)
    * [span_76](start_span)Output: $V_{out,rms}=$ (measured value) V[span_76](end_span)
    * [span_77](start_span)Frequency: $f_{m}=$ (measured value) Hz[span_77](end_span)
    * [span_78](start_span)Waveform: (Include oscilloscope capture from the physical circuit)[span_78](end_span).

### 5.3. [span_79](start_span)Analysis and Comparison[span_79](end_span)

(Student to write a paragraph comparing Theory vs. Simulation vs. Experiment. Discuss sources of error, voltage drops, and efficiency.) [span_80](start_span)

## 6. Conclusions

This module successfully demonstrated the design and operation of a single-phase full-bridge inverter[span_80](end_span). [span_81](start_span)The conversion from a DC bus to a 120V RMS, 60 Hz sinusoidal AC signal was achieved using an SPWM control strategy[span_81](end_span).

[span_82](start_span)Key learnings from this module include:[span_82](end_span)
* [span_83](start_span)The fundamental H-Bridge topology and its switching states[span_83](end_span).
* [span_84](start_span)The critical importance of dead time to prevent shoot-through[span_84](end_span).
* [span_85](start_span)The challenge of high-side gate driving and its solution using bootstrap driver ICs (IR2110)[span_85](end_span).
* [span_86](start_span)The necessity of an LC low-pass filter to extract the fundamental 60 Hz sine wave from the high-frequency PWM output[span_86](end_span).
* [span_87](start_span)The successful implementation of this inverter completes the full power conversion chain (AC-DC-AC), integrating the principles from all previous modules[span_87](end_span).

## 7. [span_88](start_span)References[span_88](end_span)

[1] Rashid, M. H. (2017). Power Electronics: Devices, Circuits, and Applications (4th ed.). [span_89](start_span)Pearson.[span_89](end_span)
[span_90](start_span)[2][span_90](end_span)