# 🧠 Gate-Level Simulation (GLS) – BabySoC  
### 🔹 Post-Synthesis Simulation Guide  

---

## 🎯 Objective  

**Gate-Level Simulation (GLS)** verifies the actual hardware-level behavior of the SoC **after synthesis**.  
Unlike RTL simulations (which operate on abstract HDL code), GLS runs on a **gate-level netlist** generated post-synthesis.  
It ensures that the synthesized circuit performs logically and meets **real-world timing requirements**.

---

## ⚙️ Key Highlights for BabySoC  

### 🔸 Functional Verification with Timing  
- GLS uses **Standard Delay Format (SDF)** to include real timing information.  
- Ensures that the circuit performs as expected under physical timing constraints.  

### 🔸 Post-Synthesis Validation  
- Confirms that the synthesized logic behaves the same as the original RTL.  
- Detects glitches, timing issues, and metastability problems before physical layout.  

### 🔸 Tools Used  
- **Yosys** – for synthesis and generating the gate-level netlist.  
- **Icarus Verilog** – for running gate-level simulation.  
- **GTKWave** – for viewing simulation waveforms.  

### 🔸 Why GLS Matters for BabySoC  
BabySoC integrates multiple components such as a **RISC-V core**, **PLL**, and **DAC**.  
GLS validates that these blocks communicate properly and that the complete system meets **timing and functional correctness**.

---

## 🧩 Step-by-Step GLS Execution Flow  

---

### 🧰 **Step 1: Load Design and Submodules**  

Open Yosys and load all required design files:  

```bash
yosys
read_verilog /home/ingenious_engineer/VSDBabysoC/src/module/vsdbabysoc.v
read_verilog -I /home/ingenious_engineer/VSDBabysoC/src/include /home/ingenious_engineer/VSDBabysoC/src/module/rvmyth.v
read_verilog -I /home/ingenious_engineer/VSDBabysoC/src/include /home/ingenious_engineer/VSDBabysoC/src/module/clk_gate.v
```
![image_alt](https://github.com/nilophertaj/CoolVSD_RTL2GDSS/blob/49ba92658a6f65ec91bea79304e6e01561fdbea1/Week%203/123yosys.png)

### 📚 Step 2: Load Liberty (.lib) Files

Load the standard cell libraries for synthesis:

```bash
read_liberty -lib /home/ingenious_engineer/VSDBabysoC/src/lib/avsdpll.lib
read_liberty -lib /home/ingenious_engineer/VSDBabysoC/src/lib/avsddac.lib
read_liberty -lib /home/ingenious_engineer/VSDBabysoC/src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
![image_alt](https://github.com/nilophertaj/CoolVSD_RTL2GDSS/blob/49ba92658a6f65ec91bea79304e6e01561fdbea1/Week%203/liberty.png)

### 🧠 Step 3: Run Synthesis for Top Module

```bash
 synth -top vsdbabysoc
```
![image_alt](https://github.com/nilophertaj/CoolVSD_RTL2GDSS/blob/49ba92658a6f65ec91bea79304e6e01561fdbea1/Week%203/Screenshot%20from%202025-10-06%2019-56-18.png
)
![image_alt](https://github.com/nilophertaj/CoolVSD_RTL2GDSS/blob/49ba92658a6f65ec91bea79304e6e01561fdbea1/Week%203/Screenshot%20from%202025-10-06%2019-58-08.png)
![image_alt]( https://github.com/nilophertaj/CoolVSD_RTL2GDSS/blob/49ba92658a6f65ec91bea79304e6e01561fdbea1/Week%203/Screenshot%20from%202025-10-06%2019-58-25.png)
![image_alt](https://github.com/nilophertaj/CoolVSD_RTL2GDSS/blob/49ba92658a6f65ec91bea79304e6e01561fdbea1/Week%203/Screenshot%20from%202025-10-06%2019-58-40.png)

### 🔄 Step 4: Map D Flip-Flops to Standard Cells
```bash
dfflibmap -liberty /home/ingenious_engineer/VSDBabysoC/src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
![image_alt](https://github.com/nilophertaj/CoolVSD_RTL2GDSS/blob/49ba92658a6f65ec91bea79304e6e01561fdbea1/Week%203/dfflib.png)

### ⚡ Step 5: Optimization and Technology Mapping
```bash
opt
abc -liberty /home/ingenious_engineer/VSDBabysoC/src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib -script +strash;scorr;ifraig;retime;{D};strash;dch,-f;map,-M,1,{D}

```
![image_alt](https://github.com/nilophertaj/CoolVSD_RTL2GDSS/blob/49ba92658a6f65ec91bea79304e6e01561fdbea1/Week%203/abc.png)

### 🧹 Step 6: Cleanup and Netlist Finalization
```bash
flatten
setundef -zero
clean -purge
rename -enumerate

```
![image_alt](https://github.com/nilophertaj/CoolVSD_RTL2GDSS/blob/49ba92658a6f65ec91bea79304e6e01561fdbea1/Week%203/final%20cleanup.png)

### 📊 Step 7: Check Design Statistics
```bash
stat
```
![image_alt](https://github.com/nilophertaj/CoolVSD_RTL2GDSS/blob/49ba92658a6f65ec91bea79304e6e01561fdbea1/Week%203/stat1.png
)
![image_alt](https://github.com/nilophertaj/CoolVSD_RTL2GDSS/blob/49ba92658a6f65ec91bea79304e6e01561fdbea1/Week%203/stat2.png)

### 💾 Step 8: Generate Synthesized Netlist
```bash
write_verilog -noattr /home/ingenious_engineer/VSDBabysoC/output/post_synth_sim/vsdbabysoc.synth.v

```
![iamge_alt](https://github.com/nilophertaj/CoolVSD_RTL2GDSS/blob/49ba92658a6f65ec91bea79304e6e01561fdbea1/Week%203/write.png)

### 🧪 Post-Synthesis Simulation & Waveform Generation
```bash
iverilog -o /home/ingenious_engineer/VSDBabysoC/output/post_synth_sim/post_synth_sim.out \
gtkwave post_synth_sim.vcd

```
![image_alt](https://github.com/nilophertaj/CoolVSD_RTL2GDSS/blob/49ba92658a6f65ec91bea79304e6e01561fdbea1/Week%203/postsim.png)

## Successfully Completed Post-Synthesis Simulation
