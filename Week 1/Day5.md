# 📘 Day -5

## Optimization in Synthesis: `if`, `case`, and Loop Constructs

This document records my **Day -5 progress** in learning:

* How **conditional constructs** (`if`, `case`) affect synthesis optimization
* How **incomplete conditions** lead to **latches** or redundant logic
* How **loops (`for`, `generate for`)** help in compact, scalable RTL design

---

## 🔹 If-Case Constructs

* `if-else` and `case` statements are widely used for conditional logic.
* Synthesizers optimize these into **multiplexers (MUXes)**.

```verilog
if (sel) 
    y = a; 
else 
    y = b; 
// Synthesizes to a 2:1 MUX
```

---

## 🔹 Incomplete If

* If an `if` block **does not assign all outputs**, synthesis may infer a **latch** to hold values.
* Bad practice for combinational logic.

```verilog
// Incomplete - latch inferred
always @(*) begin
   if (en) y = a;  // no else -> y holds previous value
end

// Correct - complete assignment
always @(*) begin
   if (en) y = a;
   else    y = b;
end
```

---

## 🔹 Incomplete & Overlapping Case

* **Incomplete case** → missing conditions → latch inferred.
* **Overlapping case** → multiple conditions true → priority mismatch in simulation/synthesis.

```verilog
// Incomplete case - latch inferred
case(sel)
   2'b00: y = a;
   2'b01: y = b;
   // missing 10, 11
endcase

// Safe way
case(sel)
   2'b00: y = a;
   2'b01: y = b;
   default: y = 0;  // prevents latch
endcase
```

✅ Use `casez`, `casex`, or `priority case` carefully to avoid ambiguity.

---

## 🔹 For Loop in RTL

* Used for **repetitive hardware structures**.
* Synthesizer unrolls loops into parallel hardware.

```verilog
// Example: 8-bit adder using loop
sum = 0;
for (i = 0; i < 8; i = i+1)
   sum = sum + a[i] * b[i];
```

---

## 🔹 Generate For

* Used to **instantiate multiple modules** or **replicate hardware**.
* Highly efficient for parameterized designs.

```verilog
genvar i;
generate 
   for (i = 0; i < 4; i = i+1) begin : gen_block
      dff dff_inst (.clk(clk), .d(d[i]), .q(q[i]));
   end
endgenerate
```

---

## 🔹 Yosys Optimization Commands

```bash
# Read RTL
read_verilog opt_constructs.v

# Run synthesis
synth -top my_design

# Clean up unused logic
opt_clean -purge

# Flatten for analysis
flatten

# Write optimized netlist
write_verilog opt_constructs_netlist.v
```

---
