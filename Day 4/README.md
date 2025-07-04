# Day 4: Gate-Level Simulation and Writing Reliable Verilog

I've continued this course to deepen my understanding of simulation practices and Verilog design reliability using open-source tools. In **Day 4** of the 5-day RTL workshop, I focused on gate-level simulation (GLS), explored the differences between blocking and non-blocking assignments in Verilog, and examined typical mismatches that can occur between simulation and synthesis results.

---

## Table of Contents

1. [Gate-Level Simulation (GLS)](#1-gate-level-simulation-gls)
2. [Synthesis vs Simulation Mismatches](#2-synthesis-vs-simulation-mismatches)
3. [Blocking vs Non-Blocking Assignments](#3-blocking-vs-non-blocking-assignments)
4. [Hands-on Labs](#4-hands-on-labs)
5. [Summary](#5-summary)

---

## 1. Gate-Level Simulation (GLS)

GLS is performed after synthesis to ensure that the netlist functions exactly as the original RTL. Unlike behavioral simulations, GLS validates the actual gates and connections generated during synthesis.

### Key Takeaways:

- Helps catch functional mismatches introduced during synthesis.
- Can be run with or without timing (functional vs timing GLS).
- Useful for verifying scan chains and testability structures.

---

## 2. Synthesis vs Simulation Mismatches

Behavioral simulation (based on RTL) and synthesized netlist simulation can yield different results if care isn't taken. These mismatches often arise from:

- Use of delays or initial blocks (which are ignored during synthesis).
- Missing sensitivity list items.
- Misuse of blocking assignments in sequential logic.

Clean, synthesizable, and deterministic RTL helps ensure consistent behavior.

---

## 3. Blocking vs Non-Blocking Assignments

In Verilog, understanding the difference between blocking (`=`) and non-blocking (`<=`) assignments is crucial.

### Blocking Assignments

- Execute statements in the exact order they're written.
- Commonly used in combinational logic.

### Non-Blocking Assignments

- Schedule updates to occur simultaneously.
- Preferred in sequential logic like flip-flops.

### Quick Comparison

| Blocking (`=`)              | Non-Blocking (`<=`)          |
| --------------------------- | ---------------------------- |
| Sequential execution        | Parallel (concurrent) update |
| Used in combinational logic | Used in sequential logic     |
| Order-sensitive             | Order-independent            |

---

## 4. Hands-on Labs

### Lab 1: Ternary MUX Using Assign Statement

```verilog
module ternary_operator_mux (input i0, input i1, input sel, output y);
  assign y = sel ? i1 : i0;
endmodule
```
<div align="center">
  <img src="https://github.com/wasee22/RTL_design_workshop_by-VSD/blob/main/Day%204/lab1.png" width="70%">
</div>
A simple 2:1 multiplexer implemented using the ternary operator.

### Lab 2: Synthesizing the MUX

Used Yosys to convert the above mux into a gate-level netlist.
<div align="center">
  <img src="https://github.com/wasee22/RTL_design_workshop_by-VSD/blob/main/Day%204/lab2.png" width="70%">
</div>

### Lab 3: GLS for the MUX

Performed gate-level simulation using the netlist and primitive libraries.

```bash
iverilog primitives.v sky130_fd_sc_hd.v mux.v tb.v
```
<div align="center">
  <img src="https://github.com/wasee22/RTL_design_workshop_by-VSD/blob/main/Day%204/lab3.png" width="70%">
</div>

### Lab 4: Incorrect MUX Example

```verilog
module bad_mux (input i0, input i1, input sel, output reg y);
  always @ (sel) begin
    if (sel)
      y <= i1;
    else 
      y <= i0;
  end
endmodule
```

**Issue:** Incomplete sensitivity list and use of non-blocking assignment in combinational logic.

**Fix:**

```verilog
always @ (*) begin
  if (sel)
    y = i1;
  else
    y = i0;
end
```
<div align="center">
  <img src="https://github.com/wasee22/RTL_design_workshop_by-VSD/blob/main/Day%204/lab4.png" width="70%">
</div>

### Lab 5: GLS of Faulty MUX

Simulated the faulty mux to observe incorrect behavior.
<div align="center">
  <img src="https://github.com/wasee22/RTL_design_workshop_by-VSD/blob/main/Day%204/lab5.png" width="70%">
</div>

### Lab 6: Blocking Assignment Pitfall

```verilog
module blocking_caveat (input a, input b, input c, output reg d);
  reg x;
  always @ (*) begin
    d = x & c;
    x = a | b;
  end
endmodule
```

**Issue:** `d` is evaluated before `x` is updated.

**Fix:**

```verilog
always @ (*) begin
  x = a | b;
  d = x & c;
end
```
<div align="center">
  <img src="https://github.com/wasee22/RTL_design_workshop_by-VSD/blob/main/Day%204/lab6.png" width="70%">
</div>

### Lab 7: Synthesizing the Fixed Version

Ran synthesis again on the corrected design to confirm expected logic structure.
<div align="center">
  <img src="https://github.com/wasee22/RTL_design_workshop_by-VSD/blob/main/Day%204/lab7.png" width="70%">
</div>
---

## 5. Summary

Day 4 reinforced the importance of writing synthesis-friendly RTL. I learned how to verify designs at the gate level, write safer combinational logic using blocking assignments, and avoid common mismatches that creep in when moving from RTL to gates.

---

