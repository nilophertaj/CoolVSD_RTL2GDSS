# 📘 Day -4

## GLS, Blocking vs Non-Blocking Assignments & Synthesis-Simulation Mismatch

This document records my **Day -4 progress** in learning:

* Performing **Gate Level Simulation (GLS)**
* Understanding **blocking (`=`) vs non-blocking (`<=`)** assignments
* Identifying **simulation vs synthesis mismatches**

---

## 🔹 Introduction to GLS

* **Gate Level Simulation (GLS)** is performed **after synthesis** using the generated **netlist**.
* It verifies whether the synthesized design (mapped to gates) behaves the same as the RTL code.
* Useful to catch mismatches that occur between **RTL simulation** and **post-synthesis logic**.

---

## 🔹 Blocking vs Non-Blocking Assignments

* **Blocking (`=`)** executes statements sequentially, like software instructions.
* **Non-blocking (`<=`)** executes concurrently, suitable for sequential logic (flops).

💡 Rule of thumb:

* Use **blocking (`=`)** in **combinational always blocks**.
* Use **non-blocking (`<=`)** in **sequential always blocks (posedge clk)**.

---

## 🔹 Simulation-Synthesis Mismatch

* Occurs when the RTL behaves differently in simulation vs synthesized gate-level design.
* Reasons include:

  * Wrong use of blocking/non-blocking assignments
  * Latches inferred due to incomplete sensitivity lists
  * Using `#delay` statements in synthesizable code

---

## 🔹 Yosys Commands for GLS Flow

```bash
# Read the Verilog design
read_verilog blocking_nonblocking.v

# Perform synthesis
synth -top my_design

# Generate gate-level netlist
write_verilog my_design_netlist.v

# Optimize (optional)
opt_clean -purge

# Re-run simulation with the netlist (GLS)
iverilog -o gls_testbench.vvp my_design_netlist.v testbench.v
vvp gls_testbench.vvp
```

---

## 🔹 Example: Identifying Mismatch

```verilog
// Wrong way - causes mismatch
always @(posedge clk) begin
  a = b;       // blocking
  c = a;       // c will get old 'a', not updated 'b'
end

// Correct way - no mismatch
always @(posedge clk) begin
  a <= b;      // non-blocking
  c <= a;      // c gets new 'a' at next clock
end
```

---

