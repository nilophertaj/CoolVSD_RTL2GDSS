# VSDBabySoC: Introduction and Design Flow

## Introduction to VSDBabySoC
VSDBabySoC is a small yet powerful **RISCV-based SoC**. The main purpose of designing such a small SoC is to test three open-source IP cores together for the first time and calibrate the analog part of it. 

VSDBabySoC contains:  
- **RVMYTH microprocessor**  
- **8x-PLL** to generate a stable clock  
- **10-bit DAC** to communicate with other analog devices  

![VSDBabySoC Block Diagram](https://github.com/nilophertaj/CoolVSD_RTL2GDSS/blob/413ac24027e2d6556ebab84509267f05fa706d04/Week%202/git/Week2%20snapshots/vsdbabysoc_block_diagram.png)

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
In this picture we can see the following signals:

- CLK: This is the input CLK signal of the RVMYTH core. This signal comes from the PLL, originally.
- reset: This is the input reset signal of the RVMYTH core. This signal comes from an external source, originally.
- OUT: This is the output OUT signal of the VSDBabySoC module. This signal comes from the DAC (due to simulation restrictions it behaves like a digital signal which is incorrect), originally.
- RV_TO_DAC[9:0]: This is the 10-bit output [9:0] OUT port of the RVMYTH core. This port comes from the RVMYTH register #17, originally.
- OUT: This is a real datatype wire which can simulate analog values. It is the output wire real OUT signal of the DAC module. This signal comes from the DAC, originally.

Note: Synthesis does not support real variables, so \vsdbabysoc.OUT behaves digitally. Use \dac.OUT to simulate analog values.

## OpenLANE Installation

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
## synthesis netlist
![image.alt](https://github.com/nilophertaj/CoolVSD_RTL2GDSS/blob/0a5c96e516af7009544a2a450e4691212b3258fd/Week%202/git/Week2%20snapshots/synth.jpg)

## post-synthesis waveform
![image.alt](https://github.com/nilophertaj/CoolVSD_RTL2GDSS/blob/0a5c96e516af7009544a2a450e4691212b3258fd/Week%202/git/Week2%20snapshots/post_synth.jpg)
In this picture we can see the following signals:

- \core.CLK: This is the input CLK signal of the RVMYTH core. This signal comes from the PLL, originally.
- reset: This is the input reset signal of the RVMYTH core. This signal comes from an external source, originally.
- OUT: This is the output OUT signal of the VSDBabySoC module. This signal comes from the DAC (due to simulation restrictions it behaves like a digital signal which is incorrect), originally.
- \core.OUT[9:0]: This is the 10-bit output [9:0] OUT port of the RVMYTH core. This port comes from the RVMYTH register #17, originally.
- OUT: This is a real datatype wire which can simulate analog values. It is the output wire real OUT signal of the DAC module. This signal comes from the DAC, originally.

Note: Synthesis does not support real variables, so \vsdbabysoc.OUT behaves digitally. Use \dac.OUT to simulate analog values.

## Yosys Final Report
![image.alt](https://github.com/nilophertaj/CoolVSD_RTL2GDSS/blob/878954409aee931ee6883bf74456755fe5c6eecc/Week%202/git/Week2%20snapshots/yorep1.png)
![image.alt](https://github.com/nilophertaj/CoolVSD_RTL2GDSS/blob/878954409aee931ee6883bf74456755fe5c6eecc/Week%202/git/Week2%20snapshots/yorep2.png)

## Static timing analysis using OpenSTA

OpenSTA is a gate level static timing verifier. As a stand-alone executable it can be used to verify the timing of a design using standard file formats. For more info about the OpenSTA see here.

Static timing analysis on the design
Due to lack of the proper PLL and DAC liberty files for complete/correct STA, we should consider the output port of the PLL (PLL.CLK) as the clock and analyze the timing of the RVMYTH core. Here is the SDC file content:

set_units -time ns
create_clock [get_pins {pll/CLK}] -name clk -period 11
Now to start the analyzing process we should do the following:

$ cd ~/VSDBabySoC
$ make sta
And now here is the output of the OpenSTA tool:
```bash
Startpoint: _9532_ (rising edge-triggered flip-flop clocked by clk)
Endpoint: _10034_ (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

  Delay    Time   Description
---------------------------------------------------------
   0.00    0.00   clock clk (rise edge)
   0.00    0.00   clock network delay (ideal)
   0.00    0.00 ^ _9532_/CLK (sky130_fd_sc_hd__dfxtp_1)
   4.40    4.40 ^ _9532_/Q (sky130_fd_sc_hd__dfxtp_1)
   5.06    9.47 v _8103_/Y (sky130_fd_sc_hd__clkinv_1)
   0.54   10.01 ^ _8106_/Y (sky130_fd_sc_hd__o211ai_1)
   0.00   10.01 ^ _10034_/D (sky130_fd_sc_hd__dfxtp_1)
          10.01   data arrival time

  11.00   11.00   clock clk (rise edge)
   0.00   11.00   clock network delay (ideal)
   0.00   11.00   clock reconvergence pessimism
          11.00 ^ _10034_/CLK (sky130_fd_sc_hd__dfxtp_1)
  -0.13   10.87   library setup time
          10.87   data required time
---------------------------------------------------------
          10.87   data required time
         -10.01   data arrival time
---------------------------------------------------------
           0.86   slack (MET)
```
## VSDBabySoC Physical Design
In integrated circuit design, physical design is a step in the standard design cycle which follows after the circuit design. At this step, circuit representations of the components (devices and interconnects) of the design are converted into geometric representations of shapes which, when manufactured in the corresponding layers of materials, will ensure the required functioning of the components. This geometric representation is called integrated circuit layout. This step is usually split into several sub-steps, which include both design and verification and validation of the layout.

## OpenLane flow
![image.alt]()

## Other required tools

Although OpenLANE integrates all required tools in its flow, sometimes we need to use a toolset direcltly from our host OS/Ubuntu. As an example it is not possible to open GUI of the Magic VLSI Layout software from the OpenLANE docker container due to container command line nature. So we need to install the tool on our main OS/Ubuntu and open it from there (not the container).

Magic
Magic is a venerable VLSI layout tool, written in the 1980's at Berkeley by John Ousterhout, now famous primarily for writing the scripting interpreter language Tcl. Due largely in part to its liberal Berkeley open-source license, magic has remained popular with universities and small companies. The open-source license has allowed VLSI engineers with a bent toward programming to implement clever ideas and help magic stay abreast of fabrication technology. However, it is the well thought-out core algorithms which lend to magic the greatest part of its popularity. Magic is widely cited as being the easiest tool to use for circuit layout, even for people who ultimately rely on commercial tools for their product design flow. Here is how to install it on a machine.

RVMYTH RTL2GDSII flow
Here we are going to implement a fully digital design using OpenLANE. This way we can get our hands dirty and learn a lot about the OpenLANE flow. Implementing mixed-signal layout without gathering knowledge of this step is pretty much tough.

RVMYTH layout generation setting up the environment
We are using OPENLANE_PATH environment variable to reference the OpenLANE installed directory. As an example imagine we have installed the OpenLANE in the ~/OpenLane directory, so the value of the OPENLANE_PATH variable would be ~/OpenLane. This value should be changed in the Makefile before any progress.

RVMYTH layout generation flow configuration
We have provided minimum required configurations in this file. However, the file could be changed accorting to other requirements. This file will be copied directly to the OpenLANE designs/rvmyth folder for layout implementation.

RVMYTH layout generation flow running
The RVMYTH layout generation flow could be all started by the following command.
```bash
$make rvmyth_layout
```
The script will take care of the rest of the process. The process should take about 20mins depending on the PC/laptop hardware configurations. After that results can be found in the output/rvmyth_layout folder.

RVMYTH post-routing simulation
The following command will produce a file named post_routing_sim.vcd which can be used to simulate post-routing and powered model.
```bash
$make rvmyth_post_routing_sim
$gtkwave out/rvmyth_layout/post_routing_sim.vcd
```
