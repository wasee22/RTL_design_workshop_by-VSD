# Day 1: Introduction to Verilog RTL Design & Synthesis

I've started this course to strengthen my understanding of digital design using open-source tools. This README captures what I learned during **Day 1** of a 5-day RTL workshop. The course walks through Verilog, simulation with **Icarus Verilog**, waveform analysis using **GTKWave**, and logic synthesis using **Yosys** along with **Sky130 PDKs**.

---

## Table of Contents

1. [Introduction to open-source simulator iverilog](#1-introduction-to-open-source-simulator-iverilog)
2. [Labs using iverilog and GTKWave](#2-labs-using-iverilog-and-gtkwave)
3. [Introduction to Yosys and Logic synthesis](#3-introduction-to-yosys-and-logic-synthesis)
4. [Labs using Yosys and Sky130 PDKs](#4-labs-using-yosys-and-sky130-pdks)
5. [Summary](#5-summary)

---

## 1. Introduction to open-source simulator iverilog

### L1: Introduction to iverilog, design, and testbench.

- A **simulator** like `iverilog` helps us verify digital logic designs by simulating their behavior.
- **Design** is the actual Verilog module describing functionality.
- **Testbench** is used to apply input and monitor outputs.

(Design and testbench files are available in the repository.)

---

## 2. Labs using iverilog and GTKWave

### L2: Lab1 Introduction to Lab

Basic setup and install of the tools.

#### Install Tools

```bash
sudo apt install iverilog
sudo apt install gtkwave
```

### L3: Lab2 Part 1 — Using iverilog & GTKWave

#### Compile and simulate:

```bash
iverilog good_mux.v tb_good_mux.v -o mux_sim
./mux_sim
```

#### View waveforms:

```bash
gtkwave tb_good_mux.vcd
```

### L4: Lab2 Part 2 — GTKWave exploration

- Open GTKWave UI
- Load `.vcd` file
- Navigate the hierarchical signals
- Zoom, scroll, and add signals to waveform viewer

<div align="center">
  <img src="https://github.com/wasee22/RTL_design_workshop_by-VSD/blob/main/Day%201/gtkwave_good_mux.png" alt="GTKWave Example" width="70%">
</div>


---

## 3. Introduction to Yosys and Logic synthesis

### L5: Introduction to Yosys

- **Yosys** is an open-source tool for logic synthesis.
- It converts your RTL code to a gate-level representation.

### L6: Logic Synthesis Part 1

#### Launch Yosys:

```bash
yosys
```

#### Read Verilog file:

```tcl
read_verilog good_mux.v
```

#### Run synthesis:

```tcl
synth -top good_mux
```

### L7: Logic Synthesis Part 2

#### Output synthesized netlist:

```tcl
write_verilog good_mux_synth.v
```

---

## 4. Labs using Yosys and Sky130 PDKs

### L8: Yosys 1 good\_mux Part 1

#### Read Sky130 Liberty file:

```tcl
read_liberty -lib /home/vsduser/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

#### Read Verilog:

```tcl
read_verilog good_mux.v
```

#### Synthesize:

```tcl
synth -top good_mux
```

### L9: Yosys 1 good\_mux Part 2

#### Map to technology library:

```tcl
abc -liberty /home/vsduser/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

#### Write synthesized netlist:

```tcl
write_verilog good_mux_mapped.v
```

### L10: Yosys 1 good\_mux Part 3

#### View synthesized schematic:

```tcl
show
```
<div align="center">
  <img src="https://github.com/wasee22/RTL_design_workshop_by-VSD/blob/main/Day%201/yosys_good_mux.png" alt="Yosys_goodmux" width="70%">
</div>
---

## 5. Summary

- Simulated Verilog using iverilog and GTKWave.
- Created and analyzed a 2-to-1 multiplexer design.
- Explored synthesis with Yosys.
- Performed technology mapping using Sky130 PDKs.
- Generated and viewed the synthesized schematic of the design.

---

Feel free to explore other days of the lab series in their respective folders!

