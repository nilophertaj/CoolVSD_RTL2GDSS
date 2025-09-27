# 📘 Day -3
## ⚡ Combinational & Sequential Logic Optimizations

This document records my **Day -3 progress** in learning:
- ✨ Introduction to **digital circuit optimization**
- 🔹 **Combinational logic optimization** techniques
- 🔹 **Sequential logic optimization** techniques
- 🔹 Optimization of **sequential logic for unused outputs**
- 📝 Implemented using **`opt_check.v`** (4 Verilog files)

📸 Snapshots of terminal commands, simulations, and synthesis outputs will be added

---

### 🔹 Introduction to Optimization

| Objective         | Description                                     |
|------------------|-------------------------------------------------|
| Reduce Area       | Minimize number of gates or logic elements     |
| Reduce Delay      | Shorten propagation time through logic         |
| Reduce Power      | Lower dynamic and static power consumption     |
| Improve Efficiency| Faster, compact, and low-power circuits        |

> Optimizations can be applied at **combinational** and **sequential** levels to improve overall design quality.

---

### 🔹 Combinational Logic Optimization ⚡

**Goal:** Minimize gates, logic depth, and improve speed.

**Techniques:**
- 🔹 Boolean simplification
- 🔹 Operator sharing (reuse intermediate signals)
- 🔹 Conditional operators (`?:`)
- 🔹 Gate-level optimizations

**Example:**
```verilog
// Original combinational logic
assign y = (a & b) | (a & c);

// Optimized combinational logic
assign y = a & (b | c);
```

### 🔹 Sequential Logic Optimization ⏱️

**Goal:** Efficient storage elements with minimal area and delay.

**Techniques:**
✅ Use non-blocking assignments (<=)
✅ Avoid unnecessary logic inside always blocks
✅ Clock gating for power saving
✅ Reduce redundant state transitions
```bash
always @(posedge clk or negedge rst_n) begin
    if (!rst_n)
        q <= 1'b0;
    else
        q <= d;
end
```

### 🔹 Sequential Logic Optimization for Unused Outputs 🛠️

| Issue                                      | Solution                              |
| ------------------------------------------ | ------------------------------------- |
| Unused outputs increase area & power       | Remove or ignore signals in synthesis |
| Unused outputs needed for constraints      | Tie outputs to constants (`0` or `1`) |
| Ensure synthesis tool ignores unused logic | Use pragma: `(* keep = "false" *)`    |

In Yosys, we can optimize them using:

opt_check – checks for optimization opportunities
opt_clean -purge – removes unused cells/signals completely

Yosys commands example (flatten multiple modules, optimize, clean, write netlist):
```bash
# Open Yosys shell
yosys

# Read multiple modules
read_verilog opt_check1.v opt_check2.v opt_check3.v opt_check4.v

# Flatten hierarchy and create optimized versions
flatten -w multiple_modules_opt*

# Perform general optimizations and unused output cleanup
opt_check
opt_clean -purge

# Write final netlist
write_verilog multiple_modules_opt_clean.v

# Exit Yosys
quit
```
