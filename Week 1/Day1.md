# 📘 Day - 1  
## Introduction to Verilog RTL Design, Testbench & Logic Synthesis  

This document records my **Day -1 progress** in learning:  
- Verilog **RTL Design**  
- **Testbenches**  
- Simulation using **Icarus Verilog (`iverilog`)**  
- Logic Synthesis using **Yosys** with Sky130 PDK  

📸 Snapshots of terminal commands and outputs will be added inline.  

---

## 🔹 What is RTL Design?  
- **RTL (Register Transfer Level) design** describes the **flow of data** between registers and the logical operations performed on that data.  
- It is written in **Verilog** or **VHDL**.  
- Example: a multiplexer, adder, or flip-flop described in Verilog.  

---

## 🔹 What is a Testbench?  
- A **testbench** is a separate Verilog file written to **verify** the RTL design.  
- It provides **stimulus (inputs)** and observes the **outputs**.  
- Helps check if the design works as expected before synthesis.  

| File            | Purpose                                             |
|-----------------|-----------------------------------------------------|
| `good_mux.v`    | RTL Design (example: 2x1 multiplexer)               |
| `tb_good_mux.v` | Testbench (stimulus + monitor for the design)       |

---
## Design Code
```bash
module good_mux (
    input  wire a,     // Input 1
    input  wire b,     // Input 2
    input  wire sel,   // Select line
    output wire y      // Output
);
assign y = sel ? b : a;
endmodule
```
## Testbench Code
```bash
`timescale 1ns/1ps
module tb_good_mux;
reg a, b, sel;
wire y;
// Instantiate the design
    good_mux uut (
        .a(a),
        .b(b),
        .sel(sel),
        .y(y)
    );
initial begin
        // Dump waveform file
        $dumpfile("tb.vcd");
        $dumpvars(0, tb_good_mux);
// Test cases
        a = 0; b = 0; sel = 0; #10;
        a = 1; b = 0; sel = 0; #10;
        a = 0; b = 1; sel = 0; #10;
        a = 1; b = 1; sel = 0; #10;
 $finish;
    end
endmodule
```

## 🔹 Icarus Verilog (iverilog)  
- **Icarus Verilog** is an open-source simulator for Verilog HDL.  
- It compiles (`iverilog`) the RTL + testbench and runs the simulation.  
- Output waveforms are viewed in **GTKWave**.  

### Commands  
```bash
iverilog good_mux.v tb_good_mux.v   # Compile design + testbench
./a.out                             # Run the simulation
gtkwave tb.vcd                      # View waveforms
```

## 🔹 Yosys & Logic Synthesis
Yosys is an open-source tool for logic synthesis.
Logic synthesis converts RTL code into a gate-level netlist using standard cell libraries (e.g., Sky130).
Steps include reading design, applying synthesis, mapping to technology cells, and writing the final netlist.

### Commands
```bash
yosys
# 1. Read your RTL design
read_verilog good_mux.v
# 2. Read the Sky130 standard cell library
read_liberty -lib /home/ingenious_engineer/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
# 3. Perform synthesis with top module
synth -top good_mux
# 4. Run technology mapping
abc -liberty /home/ingenious_engineer/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
# 5. View the synthesized logic graph
show
# 6. Write the gate-level netlist
write_verilog good_mux_netlist.v
```

### ✅ Notes:
-lib in read_liberty tells yosys it’s a library.
In abc, you again point to the same liberty file so that mapping is done to Sky130 cells.
good_mux_netlist.v will be your synthesized netlist.