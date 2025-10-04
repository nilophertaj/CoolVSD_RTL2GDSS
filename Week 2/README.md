# VSDBabySoC: Introduction and Design Flow

## Introduction to VSDBabySoC
VSDBabySoC is a small yet powerful **RISCV-based SoC**. The main purpose of designing such a small SoC is to test three open-source IP cores together for the first time and calibrate the analog part of it. 

VSDBabySoC contains:  
- **RVMYTH microprocessor**  
- **8x-PLL** to generate a stable clock  
- **10-bit DAC** to communicate with other analog devices  

![VSDBabySoC Block Diagram](vsdbabysoc_block_diagram.png)

---

## Problem Statement
This work discusses the different aspects of designing a small SoC based on RVMYTH (a RISCV-based processor). This SoC leverages a PLL as its clock generator and a 10-bit DAC to talk to the outside world.  

Applications:  
- Electrical devices like televisions and mobile phones can manipulate DAC output to provide music or video frames.  
- Fully open-source and well-documented SoC fabricated under **Sky130 technology**, suitable for educational purposes.

---

## What is SoC
An **SoC (System on Chip)** is a single-die chip that integrates multiple IP cores. These IPs could be:  
- Digital (e.g., microprocessors)  
- Analog (e.g., 5G broadband modems)

---

## What is RVMYTH
RVMYTH is a simple RISCV-based CPU created by students in a **5-day workshop** organized by RedwoodEDA and VSD.  
- Developed using **TLV (TL-Verilog)** for faster development  
- Open-source contributions managed by students  

---

## What is PLL
**Phase-Locked Loop (PLL)** is a control system that generates an output signal whose phase is related to the input signal.  
- Used for **synchronization**: clock generation and distribution  

---

## What is DAC
**Digital-to-Analog Converter (DAC)** converts a digital signal into an analog signal.  
- Enables generation of **digitally-defined transmission signals**  
- Applications: mobile communications, optical communications  

---

## VSDBabySoC Modeling
We model and simulate VSDBabySoC using **Icarus Verilog (iverilog)** and display results using **GTKWave**.  

- PLL generates **CLK**  
- RVMYTH executes instructions  
- Register **r17** values go to DAC as output (**OUT**)  

### IP Cores
1. **RVMYTH** (CPU)  
2. **PLL** (Clock Generator)  
3. **DAC** (Output to analog devices)  

---

## RVMYTH Modeling
- Written in **TL-Verilog**  
- Transformed to Verilog using **SandPiper-SaaS**  

**Reference Repo:**  
https://github.com/manili/VSDBabySoC.git

## Step-by-Step Modeling Walkthrough

### Install Required Packages
```bash
sudo apt install make python python3 python3-pip git iverilog gtkwave docker.io
sudo chmod 666 /var/run/docker.sock
cd ~
pip3 install pyyaml click sandpiper-saas
```

## Pre-synthesis Simulation
```bash
cd VSDBabySoC
make pre_synth_sim
gtkwave output/pre_synth_sim/pre_synth_sim.vcd
```
## Pre-synthesis waveform
![image.alt](https://github.com/nilophertaj/CoolVSD_RTL2GDSS/blob/0a5c96e516af7009544a2a450e4691212b3258fd/Week%202/git/Week2%20snapshots/Pre_synth.png)
Important Signals:

- CLK: PLL output
- OUT: DAC output
- RV_TO_DAC[9:0]: RVMYTH register #17

Note: Synthesis does not support real variables, so \vsdbabysoc.OUT behaves digitally. Use \dac.OUT to simulate analog values.

OpenLANE Installation

OpenLANE automates RTL to GDSII flow.
Installation:
```bash
git clone https://github.com/The-OpenROAD-Project/OpenLane.git
cd OpenLane/
make openlane
make pdk
make test
```

## Post-synthesis Simulation
```bash
cd ~/VSDBabySoC
make synth
make post_synth_sim
gtkwave output/post_synth_sim/post_synth_sim.vcd
```
## synthesis waveform
![image.alt](https://github.com/nilophertaj/CoolVSD_RTL2GDSS/blob/0a5c96e516af7009544a2a450e4691212b3258fd/Week%202/git/Week2%20snapshots/synth.jpg)

## post-synthesis waveform
![image.alt](https://github.com/nilophertaj/CoolVSD_RTL2GDSS/blob/0a5c96e516af7009544a2a450e4691212b3258fd/Week%202/git/Week2%20snapshots/post_synth.jpg)

Signals:

- \core.CLK: RVMYTH clock input
- reset: RVMYTH reset input
- \core.OUT[9:0]: 10-bit output

