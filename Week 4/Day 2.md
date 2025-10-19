# ⚡ Velocity Saturation & CMOS Inverter VTC

## 🚀 Velocity Saturation

When the **electric field** in a MOSFET channel becomes very high (typically above **10⁴ V/cm**), the **carrier velocity** no longer increases linearly with the electric field.  
Instead, it reaches a **maximum limit**, known as the **saturation velocity (v<sub>sat</sub>)**.

### 🔹 Key Concept:
- At **low electric fields**, electron velocity `v` increases linearly with the field:
  \[
  v = \mu E
  \]
  where  
  - `μ` = carrier mobility  
  - `E` = electric field  

- At **high electric fields**, velocity reaches its limit:
  \[
  v = v_{sat}
  \]

### 🔹 Effect on Drain Current:
Due to velocity saturation, the **drain current** no longer follows the ideal quadratic law.  
Instead, it becomes **linearly dependent** on `(V<sub>GS</sub> − V<sub>T</sub>)`:

\[
I_D = W \cdot C_{ox} \cdot v_{sat} \cdot (V_{GS} - V_T)
\]

where  
- `W` = channel width  
- `Cₒₓ` = oxide capacitance per unit area  
- `V_GS` = gate-to-source voltage  
- `V_T` = threshold voltage  

### 🔹 Importance:
- Occurs in **deep submicron technologies** (like Sky130, 180nm, 90nm, etc.)  
- Limits transistor speed improvement despite scaling.  
- Impacts **transconductance (gₘ)** and **gain** in analog circuits.  
- Must be considered in **high-frequency** and **low-channel-length** designs.

---

## ⚙️ CMOS Inverter VTC (Voltage Transfer Characteristics)

A **CMOS inverter** consists of a **pMOS** and **nMOS** transistor connected in a complementary way.  
Its **Voltage Transfer Characteristic (VTC)** shows the relationship between the **input voltage (V<sub>in</sub>)** and the **output voltage (V<sub>out</sub>)**.

### 🔹 Basic Operation Regions:
| Input Voltage | nMOS | pMOS | Output |
|----------------|------|------|---------|
| Low (0V) | OFF | ON | High (≈ V<sub>DD</sub>) |
| High (V<sub>DD</sub>) | ON | OFF | Low (≈ 0V) |
| Mid Range | Both Partially ON | Both Partially ON | Transition Region |

### 🔹 Important Points in VTC:
1. **VOH (Output High Voltage):** ≈ V<sub>DD</sub>  
2. **VOL (Output Low Voltage):** ≈ 0V  
3. **VIL, VIH:** Input voltage levels at which output starts to change significantly.  
4. **VM (Switching Threshold):** The point where V<sub>in</sub> = V<sub>out</sub>  
   - At this point, both transistors conduct equally.  
5. **Noise Margins:**  
   \[
   NM_H = VOH - VIH
   \]
   \[
   NM_L = VIL - VOL
   \]

### 🔹 VTC Graph Description:
The **VTC curve** starts high (VOH), drops sharply at the **transition region**, and finally settles low (VOL).  
A steep transition indicates **good noise immunity** and **strong inverter switching behavior**.

---

### 📘 Summary:
- **Velocity Saturation**: Limits carrier velocity and affects MOSFET current behavior.  
- **CMOS Inverter VTC**: Explains inverter switching response and noise immunity.  

Both are crucial for **deep-submicron CMOS design** and **SPICE simulations**.

---

### 🧠 Tip:
When simulating in **NGSPICE**, you can observe:
- The **Id–Vds** curve flattening due to velocity saturation.
- The **VTC plot** showing the inverter switching threshold.

## Commands
```bash
vim day2_idvds.spice
```
![image_alt](https://github.com/nilophertaj/CoolVSD_RTL2GDSS/blob/1d5f46eec27b4952cc2445e9d5889fa5e09719c4/Week%204/assets/day2%20idvds.png)
## Line-line by explanation
🔹- .param temp=27
Sets the simulation temperature to 27°C (room temperature).

🔹- .lib "sky130_fd_pr/models/sky130.lib.spice" tt
Includes the SKY130 PDK model file which contains transistor characteristics.
The tt stands for typical-typical corner, meaning average process conditions for NMOS and PMOS devices.

🔹- XM1 Vdd n1 0 0 sky130_fd_pr__nfet_01v8 w=0.39 l=0.15
Defines the NMOS transistor M1.
Node connections:
Drain → Vdd
Gate → n1
Source → 0 (ground)
Bulk → 0 (ground)
Model name: sky130_fd_pr__nfet_01v8 → standard 1.8 V NMOS device from the SKY130 library.
W=0.39 µm, L=0.15 µm define the transistor’s width and length.

🔹- R1 n1 in 55
A resistor of 55 Ω connects node n1 (the gate of the MOSFET) to the input node in.
It provides biasing or stabilizes the gate voltage.

🔹- Vdd vdd 0 1.8V
DC supply voltage source of 1.8 V connected between node vdd and ground.
This acts as the drain bias.

🔹- Vin in 0 1.8V
Input voltage source connected between node in and ground.
It controls the gate voltage (V<sub>GS</sub>) of the NMOS transistor.

🔹- .op
Performs an operating point analysis, calculating DC bias points for all nodes and currents.

🔹- .dc Vdd 0 1.8 0.1 Vin 0 1.8 0.2
Runs a DC sweep analysis:
Sweeps Vdd from 0 V to 1.8 V in steps of 0.1 V.
For each value of Vdd, it also sweeps Vin from 0 V to 1.8 V in steps of 0.2 V.
This helps generate I<sub>D</sub>–V<sub>DS</sub> or I<sub>D</sub>–V<sub>GS</sub> plots.

🔹- .control ... .endc
These commands are interactive instructions for NGSPICE’s internal terminal.
run → starts the simulation.
display → shows available variables (voltages and currents).
setplot dc1 → selects the DC analysis plot to visualize results.

🔹- .end
Marks the end of the SPICE file.

## Characteristics of idvds
```bash
ngspice day2_idvds.spice
plot -vdd#branch
```
![image_alt](https://github.com/nilophertaj/CoolVSD_RTL2GDSS/blob/1d5f46eec27b4952cc2445e9d5889fa5e09719c4/Week%204/assets/plot%20idvds.png)

## Characteristics of idvgs
```bash
ngspice day2_idvgs.spice
plot -vdd#branch
```
![image_alt](https://github.com/nilophertaj/CoolVSD_RTL2GDSS/blob/1d5f46eec27b4952cc2445e9d5889fa5e09719c4/Week%204/assets/idvgs.png
)
![image_alt](https://github.com/nilophertaj/CoolVSD_RTL2GDSS/blob/1d5f46eec27b4952cc2445e9d5889fa5e09719c4/Week%204/assets/plot%20idvgs.png)

## How to calculate threshold voltage (Vth) in Id Vs Vgs ? 
- Move the cursor along the tangent line towards down on X-axis.
- That point from X-axis is known as the threshold point/threshold voltage.

  Below proof shows you that the threshold values for Id Vs Vgs plot
  ![image_alt](https://github.com/nilophertaj/CoolVSD_RTL2GDSS/blob/1d5f46eec27b4952cc2445e9d5889fa5e09719c4/Week%204/assets/vto.png)
