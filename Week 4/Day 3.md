# ⚡ CMOS Switching Threshold & Dynamic Simulations

## 🧩 CMOS Switching Threshold (V<sub>M</sub>)

The **switching threshold** (V<sub>M</sub>) of a CMOS inverter is the **input voltage** at which the **output voltage equals the input voltage**.  
At this point, **both nMOS and pMOS transistors conduct equal current**.

### 🔹 Definition:
\[
V_M = V_{in} = V_{out}
\]

This is the **balance point** of the inverter’s transfer characteristic curve.

### 🔹 Conceptual Explanation:
- When **V<sub>in</sub> < V<sub>M</sub>**, pMOS conducts strongly, pulling the output HIGH.  
- When **V<sub>in</sub> > V<sub>M</sub>**, nMOS conducts strongly, pulling the output LOW.  
- At **V<sub>in</sub> = V<sub>M</sub>**, both transistors are in **saturation region**, and  
  \[
  I_{Dn} = I_{Dp}
  \]
  which gives:
  \[
  k_n (V_M - V_{Tn})^2 = k_p (V_{DD} - V_M - |V_{Tp}|)^2
  \]
  Solving this equation gives the **switching threshold voltage (V<sub>M</sub>)**.

### 🔹 Importance:
- Determines the **noise margins** of the inverter.  
- Affects **logic level restoration** and **signal stability**.  
- Ideally, **V<sub>M</sub> ≈ V<sub>DD</sub>/2** for symmetrical operation.

---

## ⚙️ Dynamic Simulations

Dynamic simulations analyze how the **inverter responds to changing input signals** (transient analysis).  
It helps observe **switching speed, propagation delay, rise/fall times**, and **power consumption**.

### 🔹 Transient (Time-Domain) Simulation in NGSPICE:

To perform a transient analysis:
```bash
tran 0.1n 10n
plot v(in) v(out)

```
🔹 Key Observations:

Propagation Delay (t<sub>pd</sub>):
Time taken for output to switch after input transition.

Rise Time (t<sub>r</sub>):
Time taken for output to rise from 10% to 90% of V<sub>DD</sub>.

Fall Time (t<sub>f</sub>):
Time taken for output to fall from 90% to 10% of V<sub>DD</sub>.

🔹 Formula:
\[
P_{dyn} = C_L \cdot V_{DD}^2 \cdot f
\]

### 🔹 Where:
- **C<sub>L</sub>** → Load capacitance  
- **V<sub>DD</sub>** → Supply voltage  
- **f** → Switching frequency  

📉 Voltage Transfer Characteristics (VTC)

The VTC curve represents the relationship between V<sub>out</sub> and V<sub>in</sub> of a CMOS inverter.
| Region     | nMOS         | pMOS         | Output                  |
| ---------- | ------------ | ------------ | ----------------------- |
| Region I   | OFF          | ON           | HIGH (≈ V<sub>DD</sub>) |
| Region II  | Partially ON | Partially ON | Transition Region       |
| Region III | ON           | OFF          | LOW (≈ 0V)              |

🔹 Graph Description:

Starts high (VOH ≈ V<sub>DD</sub>), drops sharply near V<sub>M</sub>, then stabilizes low (VOL ≈ 0V).

The steeper the curve, the better the noise immunity and inverter switching behavior.

🔹 Important Points:

VOH (Output High Voltage): ≈ V<sub>DD</sub>

VOL (Output Low Voltage): ≈ 0V

VIL, VIH: Input limits of logic transition

VM (Switching Threshold): Midpoint of transition

### 🔹 Equations:
\[
NM_H = VOH - VIH
\]
\[
NM_L = VIL - VOL
\]

🧠 Static Behavior Evaluation

Static behavior represents the steady-state response of the inverter for constant input voltages.
It defines the logic stability and noise immunity of the CMOS circuit.
| Parameter                   | Meaning                                   |
| --------------------------- | ----------------------------------------- |
| **VOH**                     | Maximum logic high output voltage         |
| **VOL**                     | Minimum logic low output voltage          |
| **VIL**                     | Max input voltage recognized as logic ‘0’ |
| **VIH**                     | Min input voltage recognized as logic ‘1’ |
| **Noise Margin High (NMH)** | VOH − VIH                                 |
| **Noise Margin Low (NML)**  | VIL − VOL                                 |

A higher noise margin ensures reliable logic operation under voltage fluctuations and noise.

## Summary
| Concept                                    | Description                                                     |
| ------------------------------------------ | --------------------------------------------------------------- |
| **Switching Threshold (V<sub>M</sub>)**    | Input voltage where output equals input                         |
| **VTC (Voltage Transfer Characteristics)** | Graph showing V<sub>out</sub> vs V<sub>in</sub>                 |
| **Static Behavior**                        | Defines steady-state noise margins and logic stability          |
| **Dynamic Simulation**                     | Observes inverter delay, rise/fall time, and switching response |
