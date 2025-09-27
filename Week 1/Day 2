# 📘 Day -2  
## Timing Libraries, Hierarchical vs Flat Synthesis, and Efficient Flop Coding Styles  

This document records my **Day -2 progress** in learning:  
- Understanding **timing libraries (`.lib` files)**  
- Exploring **hierarchical vs flat synthesis**  
- Learning about **flops, flop synthesis, and coding styles**  
- Performing synthesis of multi-module designs  
- Generating separate **netlists for submodules**  

📸 Snapshots of terminal commands and outputs will be added inline.  

---

## 🔹 Timing Libraries (`.lib`)  
- A **`.lib` file** defines the **characteristics of standard cells** (gates, flops, mux, etc.) in a particular technology.  
- Contains details like:  
  - Propagation delay  
  - Setup/Hold times  
  - Power consumption  
  - Area  
- Used by **Yosys** and **ABC** to map RTL → technology-specific gates.  

📸 *Insert snapshot of opened `.lib` here.*  

---

## 🔹 Flops and Flop Synthesis  
- **Flip-flops (flops)** are sequential elements essential for designing registers, counters, and pipelines.  
- **Flop synthesis** ensures correct mapping of RTL-coded sequential logic into library-supported flip-flop cells.  

### Flop Coding Styles  
- **Synchronous reset** (preferred in most designs):  
  ```verilog
  always @(posedge clk) begin
      if (rst)
          q <= 0;
      else
          q <= d;
  end
```
- **Asynchronous reset**
```verilog
always @(posedge clk or posedge rst) begin
    if (rst)
        q <= 0;
    else
        q <= d;
end
```

### Interesting Optimizations
- Unused logic removal
- Constant propagation
- Resource sharing (reusing same hardware for multiple operations)
- Dead-code elimination

| File                 | Purpose                                    |
| -------------------- | ------------------------------------------ |
| `multiple_modules.v` | RTL design containing multiple sub-modules |

🔹 Yosys Synthesis Flow
Step 1: Read RTL Design
```bash
yosys
#inside yosys shell
read_verilog multiple_modules.v
read_liberty -lib /home/ingenious_engineer/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib

Step 2: Hierarchical Synthesis
Keeps the design hierarchy (separate modules remain visible).
synth -top top_module
write_verilog multiple_modules_hier_netlist.v

Step 3: Flat Synthesis
Flattens all modules into a single-level netlist.
flatten
synth -top top_module
write_verilog multiple_modules_flat_netlist.v

Step 4: Technology Mapping with ABC
abc -liberty /home/ingenious_engineer/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show

🔹 Why Submodule-Level (Hierarchical) Synthesis?

Modularity & Debugging
- By synthesizing each submodule separately, you can check if the RTL for that block works properly before integrating.
- Helps in debugging issues locally instead of searching across the entire design.

