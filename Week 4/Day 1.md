# 🧠 CMOS Circuit Design & SPICE Simulation (NMOS Characterization)

This repository covers the **simulation and analysis of NMOS transistor operation** using **NGSPICE** under various regions — from resistive to saturation — including theoretical background, threshold voltage concepts, and SPICE simulation commands executed in Linux (Ubuntu).

---

## ⚙️ 1. Introduction to Circuit Design & SPICE Simulations

The basic NMOS circuit used for simulation consists of:

- **VDD = 2.5V**
- **Resistor R1 = 550 Ω**
- **Transistor M1 (NMOS): W/L = 1.8µ / 1.2µ**
- **Input voltage (Vin) = 2.5V**

**Circuit Diagram Reference:**
![NMOS_Circuit](https://github.com/nilophertaj/CoolVSD_RTL2GDSS/blob/57f24c0c4797d8b7d0dcf21295a38f774191ad77/Week%204/assets/WhatsApp%20Image%202025-10-19%20at%2017.38.40_7210f74d.jpg)

### Simulation Parameters:
- \( V_{DD} = 2.5V \)
- \( V_{DS} \): Sweep from **0 → 2.5V**
- \( V_{in} \): Applied as gate voltage

---

## ⚡ 2. Strong Inversion & Threshold Voltage Concepts

### Threshold Voltage (\(V_T\))

The **threshold voltage** is the minimum gate voltage at which an inversion layer forms at the semiconductor surface.

\[
V_T = V_{TO} + \gamma \left( \sqrt{2\phi_F + V_{SB}} - \sqrt{2\phi_F} \right)
\]

Where:
- \(V_{TO}\): Zero-bias threshold voltage  
- \(\gamma\): Body effect coefficient  
- \(\phi_F\): Fermi potential  
- \(V_{SB}\): Source-to-body voltage  

---

## 🔋 3. NMOS Regions of Operation

### (a) Resistive / Linear Region

Occurs when \( V_{DS} < (V_{GS} - V_T) \).

**Condition:**
\[
V_{DS} < V_{GS} - V_T
\]

**Drain Current Equation:**
\[
I_D = \mu_n C_{ox} \frac{W}{L} \left[ (V_{GS} - V_T)V_{DS} - \frac{V_{DS}^2}{2} \right]
\]

**Concepts:**
1. **Drift Current Theory:**  
   Current is mainly due to drift of electrons under electric field \( E \).

2. **Drain Current Model:**  
   Based on gradual channel approximation, voltage varies linearly from source to drain.

3. **SPICE Observation:**  
   For small \(V_{DS}\), the NMOS acts as a **voltage-controlled resistor**.

---

### (b) Saturation / Pinch-Off Region

Occurs when \( V_{DS} \geq (V_{GS} - V_T) \).

**Condition:**
\[
V_{DS} \geq V_{GS} - V_T
\]

**Drain Current Equation:**
\[
I_D = \frac{1}{2} \mu_n C_{ox} \frac{W}{L} (V_{GS} - V_T)^2
\]

**Concept:**
- The channel is pinched off near the drain.
- Current becomes **independent of \(V_{DS}\)** and controlled only by \(V_{GS}\)**.

---

## 🧩 4. Introduction to NGSPICE

**NGSPICE** is an open-source circuit simulator that allows transistor-level simulations and visualization of I–V plots.

### Basic SPICE Setup:
- Each device and parameter is described in **SPICE netlist format**.
- Run simulations via Linux terminal using commands.

---

## 🧠 5. Circuit Description in SPICE Syntax

Example netlist for NMOS I–V simulation:

```spice
* NMOS IV Characteristics
M1 D G S 0 nmos W=1.8u L=1.2u
VDD D 0 2.5
VIN G 0 2.5
VSS S 0 0
R1 G n1 550
.dc VDD 0 2.5 0.05
.model nmos NMOS (Level=1 VTO=0.7 KP=120e-6)
.end
```
## Commands
```spice
# after cloning kunalg123 repo sky130CircuitDesignWorkshop using 'git clone ***'
cd sky130CircuitDesignWorkshop/design
ls
#cd to the directory under design
ls
cd cells/nfet***/
less sky130_fd_pr__nfet_01v8_tt.pm3.spice
less sky130_fd_pr__nfet_01v8_tt.corner.spice
```
![image_alt](https://github.com/nilophertaj/CoolVSD_RTL2GDSS/blob/864b2b63406d3bd3ed6bef0e0948a771a3ce40ff/Week%204/assets/day1cmds1.png)
![image_alt](https://github.com/nilophertaj/CoolVSD_RTL2GDSS/blob/864b2b63406d3bd3ed6bef0e0948a771a3ce40ff/Week%204/assets/nfet_tt.png)
- It will describes about the nmos technology typical node in spice file.
  
![image_alt](https://github.com/nilophertaj/CoolVSD_RTL2GDSS/blob/864b2b63406d3bd3ed6bef0e0948a771a3ce40ff/Week%204/assets/day1cmds2.png
)
![image_alt](https://github.com/nilophertaj/CoolVSD_RTL2GDSS/blob/864b2b63406d3bd3ed6bef0e0948a771a3ce40ff/Week%204/assets/nfet_corners.png)
- It will describes about the different values of W(Width) & L(Length) for different nmos structures.
  
  ```spice
  cd ../../models/
  ls
  less sky130.lib.sp
  ```
  ![image_alt](https://github.com/nilophertaj/CoolVSD_RTL2GDSS/blob/864b2b63406d3bd3ed6bef0e0948a771a3ce40ff/Week%204/assets/ttsffs.png)
  - It contains different tt(typical), sf(slow-fast), fs(fast-slow),etc values for the technology node.
    ```spice
    # move to the design directory
    clear
    ls
    vim
    ```
    ![image_alt](https://github.com/nilophertaj/CoolVSD_RTL2GDSS/blob/864b2b63406d3bd3ed6bef0e0948a771a3ce40ff/Week%204/assets/day1.png)
    - This file contains the Model parameter, Model description, Including .lib files, Netlist description, Simulation commands.
      ```spice
      #invoke ngspice simulation
      ngspice
      #inside ngspice shell
      plot -vdd#branch
      # this will plot the Id Vs Vgs graph
      ```
      ![image_alt](https://github.com/nilophertaj/CoolVSD_RTL2GDSS/blob/864b2b63406d3bd3ed6bef0e0948a771a3ce40ff/Week%204/assets/day1plot.png)
      - It has shown the curve between Id Vs Vgs graph.
      ## 🔍 Key Observations

### **Cutoff Region (V<sub>GS</sub> < V<sub>T</sub>):**
- When **V<sub>GS</sub>** is less than the **threshold voltage (V<sub>T</sub>)**, the MOSFET is **OFF**.  
- The channel doesn’t form, and **I<sub>D</sub> ≈ 0**.  
- The MOSFET acts as an open switch in this region.

---

### **Linear / Triode Region (V<sub>GS</sub> > V<sub>T</sub>, small V<sub>DS</sub>):**
- As **V<sub>GS</sub>** exceeds **V<sub>T</sub>**, a channel forms and allows electrons to flow.  
- The current increases **linearly** with **V<sub>GS</sub>**.  
- The MOSFET behaves like a **voltage-controlled resistor**.  

**Equation:**  
\( I_D = k[(V_{GS} - V_T)V_{DS} - \frac{V_{DS}^2}{2}] \)

---

### **Saturation Region (V<sub>GS</sub> > V<sub>T</sub>, large V<sub>DS</sub>):**
- Beyond a certain point, the channel near the drain **pinches off**.  
- The current increases **quadratically** with **V<sub>GS</sub>** and becomes **independent of V<sub>DS</sub>**.  

**Equation:**  
\( I_D = \frac{1}{2}k(V_{GS} - V_T)^2 \)


