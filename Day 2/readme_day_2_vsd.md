# Day 2: Timing Libraries, Synthesis Approaches, and Efficient Flip-Flop Coding

I've continued this course to expand my knowledge of RTL design using open-source tools. This README summarizes what I learned during **Day 2** of the 5-day RTL workshop. Today’s focus was on timing libraries, hierarchical vs. flat synthesis approaches, and efficient flip-flop coding styles.

---

## Table of Contents

1. [Introduction to timing .lib files](#1-introduction-to-timing-lib-files)
2. [Hierarchical vs. Flat Synthesis](#2-hierarchical-vs-flat-synthesis)
3. [Flop Coding Styles and Optimization](#3-flop-coding-styles-and-optimization)
4. [Summary](#4-summary)

---

## 1. Introduction to timing .lib files

### L1: Introduction to .lib Part 1

The SKY130 PDK provides `.lib` files like `sky130_fd_sc_hd__tt_025C_1v80.lib`, which contain standard cell information under specific process-voltage-temperature (PVT) conditions.

- **tt** → typical process
- **025C** → temperature = 25°C
- **1v80** → voltage = 1.8V

### L2: Introduction to .lib Part 2

To explore the `.lib` file:

#### Install Gedit (Text Editor)

```bash
sudo apt install gedit
```

#### Open the Library File

```bash
gedit sky130_fd_sc_hd__tt_025C_1v80.lib
```

You can inspect details like cell delays, setup/hold times, and power information.

### L3: Introduction to .lib Part 3

This part involved understanding how synthesis tools use this file during technology mapping and optimization.

---

## 2. Hierarchical vs Flat Synthesis

### L4: Hierarchical Synthesis Part 1

Hierarchical synthesis retains the module structure as defined in RTL. Each sub-module is synthesized independently, allowing modular analysis. Yosys uses commands like `hierarchy` to analyze and organize the design:

```tcl
hierarchy -check -top top_module
```

### L5: Hierarchical Synthesis Part 2

This continues the exploration of how synthesis tools interpret and maintain the hierarchical structure across modules. This method is useful for modular debugging and supports incremental design changes.

### L6: Flattened Synthesis

In flattened synthesis, all modules are merged into a single-level netlist. This eliminates the original hierarchy and enables whole-design optimization. The process in Yosys involves:

```tcl
flatten top_module
```

Flattening results in a single-level gate netlist that can improve synthesis optimizations across the entire design.

---

## 3. Flop Coding Styles and Optimization

### L7: Why Flops and Coding Styles Part 1

Explains why flip-flops are used in sequential logic, including basic types of resets and sets. They are essential in preventing glitches and hazards that typically occur in purely combinational circuits.

### L8: Why Flops and Coding Styles Part 2

This section covers additional details and usage styles of flip-flops, along with their Verilog implementations.

#### Asynchronous Reset D Flip-Flop

```verilog
module dff_asyncres (input clk, input async_reset, input d, output reg q);
  always @ (posedge clk, posedge async_reset)
    if (async_reset)
      q <= 1'b0;
    else
      q <= d;
endmodule
```

- This flip-flop asynchronously resets its output to 0 whenever `async_reset` goes high, regardless of the clock.
- When `async_reset` is low, it stores the value of `d` on the rising edge of the clock.

#### Asynchronous Set D Flip-Flop

```verilog
module dff_async_set (input clk, input async_set, input d, output reg q);
  always @ (posedge clk, posedge async_set)
    if (async_set)
      q <= 1'b1;
    else
      q <= d;
endmodule
```

- Similar to the asynchronous reset flip-flop, this one sets the output to 1 immediately when `async_set` is triggered.
- Otherwise, it samples the input `d` on the rising clock edge.

#### Synchronous Reset D Flip-Flop

```verilog
module dff_syncres (input clk, input async_reset, input sync_reset, input d, output reg q);
  always @ (posedge clk)
    if (sync_reset)
      q <= 1'b0;
    else
      q <= d;
endmodule
```

- In this case, the reset signal only affects the output during the rising edge of the clock.
- This makes it suitable for designs where reset behavior must be synchronized with clock transitions.

### L9: Flop Synthesis Simulation Part 1

All three flip-flop designs are simulated using Icarus Verilog and waveform viewing in GTKWave.

#### Commands:

```bash
iverilog dff_asyncres.v tb_dff_asyncres.v
./a.out
gtkwave tb_dff_asyncres.vcd

iverilog dff_asyncset.v tb_dff_asyncset.v
./a.out
gtkwave tb_dff_async_set.vcd

iverilog dff_syncres.v tb_dff_syncres.v
./a.out
gtkwave tb_dff_syncres.vcd
```

(Screenshots of the waveforms will be linked using GitHub URLs.)

### L10: Flop Synthesis Simulation Part 2

This part focuses on synthesizing all three designs using Yosys. Below are the standard commands:

#### Synthesis with Yosys

```bash
yosys
```

#### Read Liberty library

```tcl
read_liberty -lib /home/vsduser/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

#### Read Verilog code

```tcl
read_verilog dff_asyncres.v
```

#### Synthesize

```tcl
synth -top dff_asyncres
```

#### Map flip-flops

```tcl
dfflibmap -liberty /home/vsduser/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

#### Technology mapping

```tcl
abc -liberty /home/vsduser/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

#### Visualize the gate-level netlist

```tcl
show
```

(Respective screenshots of each flip-flop's gate-level view will be added as GitHub image links.)

---

## 4. Summary

Day 2 focused on understanding timing libraries, exploring synthesis styles, and coding flip-flops efficiently. I practiced simulations and synthesis using open-source tools like Icarus Verilog, GTKWave, and Yosys, further solidifying my RTL design skills.

---

