# Day 3: Combinational and Sequential Optimization

I've continued this course to expand my knowledge of RTL design using open-source tools. This README summarizes what I learned during **Day 3** of the 5-day RTL workshop. Today’s focus was on optimization techniques for combinational and sequential circuits to improve efficiency, performance, and logic quality.

---

## Table of Contents

1. [Constant Propagation](#1-constant-propagation)
2. [State Optimization](#2-state-optimization)
3. [Cloning](#3-cloning)
4. [Retiming](#4-retiming)
5. [Labs on Optimization](#5-labs-on-optimization)
6. [Summary](#6-summary)

---

## 1. Constant Propagation

Constant propagation is a compiler optimization technique used to simplify logic by replacing variables with constant values at synthesis time.

**How it works:**\
The tool analyzes the code to detect constant-valued expressions or conditions. These are replaced directly, reducing circuit size and logic complexity.

**Benefits:**

- Simpler logic, fewer gates
- Improved performance due to shorter critical paths
- Reduced power consumption



---

## 2. State Optimization

State optimization enhances finite state machines (FSMs) by reducing redundant states, improving encoding, and minimizing transition logic.

**Approaches include:**

- Merging equivalent states
- Re-encoding states for simpler transitions
- Using Boolean algebra to reduce logic
- Applying power-aware techniques like clock gating

---

## 3. Cloning

Cloning involves duplicating logic blocks to optimize timing, reduce load capacitance, or improve routing.

**Steps:**

- Identify timing bottlenecks
- Duplicate logic elements
- Re-distribute loads
- Validate performance improvement



---

## 4. Retiming

Retiming repositions registers in a circuit to balance logic depth and improve timing.

**Process:**

- Represent the circuit as a graph
- Move registers across logic gates without changing functionality
- Optimize clock period and reduce timing violations

---

## 5. Labs on Optimization

### Lab 1

Below is the Verilog code for Lab 1:

```verilog
module opt_check (input a , input b , output y);
	assign y = a?b:0;
endmodule
```

**Explanation:**

- `assign y = a ? b : 0;` means:
  - If `a` is true, `y` is assigned the value of `b`.
  - If `a` is false, `y` is 0.

This lab also demonstrates the use of the Yosys optimization command:

```tcl
opt_clean -purge
```

which helps remove redundant logic and simplify the final gate-level netlist.
<div align="center">
  <img src="https://github.com/wasee22/RTL_design_workshop_by-VSD/blob/main/Day%203/opt.png" width="70%">
</div>


---

### Lab 2

Verilog code:

```verilog
module opt_check2 (input a , input b , output y);
	assign y = a?1:b;
endmodule
```

**Code Analysis:**

- Acts as a multiplexer:
  - `y = 1` if `a` is true.
  - `y = b` if `a` is false.

<div align="center">
  <img src="https://github.com/wasee22/RTL_design_workshop_by-VSD/blob/main/Day%203/op2.png" width="70%">
</div>

---

### Lab 3

Verilog code:

```verilog
module opt_check2 (input a , input b , output y);
	assign y = a?1:b;
endmodule
```

**Functionality:**\
2-to-1 multiplexer; `y = a ? 1 : b` (outputs `1` when `a` is true, otherwise `b`).

<div align="center">
  <img src="https://github.com/wasee22/RTL_design_workshop_by-VSD/blob/main/Day%203/opt3.png" width="70%">
</div>

---

### Lab 4

Verilog code:

```verilog
module opt_check4 (input a , input b , input c , output y);
 assign y = a?(b?(a & c ):c):(!c);
endmodule
```

**Functionality:**

- Three inputs (`a`, `b`, `c`), output `y`.
- Nested ternary logic:
  - If `a = 1`, `y = c`.
  - If `a = 0`, `y = !c`.
- Logic simplifies to:\
  `y = a ? c : !c`

<div align="center">
  <img src="https://github.com/wasee22/RTL_design_workshop_by-VSD/blob/main/Day%203/opt4.png" width="70%">
</div>

---

### Lab 5

Verilog code:

```verilog
module dff_const1(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
	if(reset)
		q <= 1'b0;
	else
		q <= 1'b1;
end
endmodule
```

**Functionality:**

- D flip-flop with:
  - Asynchronous reset to 0
  - Loads constant `1` when not in reset


<div align="center">
  <img src="https://github.com/wasee22/RTL_design_workshop_by-VSD/blob/main/Day%203/dff_const1.png" width="70%">
</div>
---

### Lab 6

Verilog code:

```verilog
module dff_const2(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
	if(reset)
		q <= 1'b1;
	else
		q <= 1'b1;
end
endmodule
```

**Functionality:**

- D flip-flop always sets output `q` to `1` (regardless of reset or clock).

<div align="center">
  <img src="https://github.com/wasee22/RTL_design_workshop_by-VSD/blob/main/Day%203/dff_const2.png" width="70%">
</div>

---

## 6. Summary

Day 3 focused on optimization techniques for digital circuits. I explored constant propagation, state optimization, cloning, and retiming—alongside hands-on labs using Verilog and Yosys to apply these concepts practically.

---

