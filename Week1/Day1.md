# Introduction to Verilog RTL Design & Synthesis

----

### Intro to Iverilog  
- **Iverilog**  
  : Used for writing and running **designs** and **test benches**.  

- **Simulation**  
  : Always evaluates output **only when there is any change in the input** detected.  

- **Gtkwave**  
  : Used to visualize simulation **waveforms**.  

---

### Introduction to Yosys  
- **Synthesis** = Conversion of **RTL** into **netlist**.  

**Yosys commands**:  
- `read_verilog` → Reads the design file  
- `read_liberty` → Reads `.lib` (standard cell library) files  
- `write_verilog` → Generates **netlist files**  

**To verify synthesis**:  
- Give the **netlist** and the **test bench** (same TB for both RTL design and synthesized netlists) to **Iverilog**  
- Observe the waveforms using **Gtkwave**  

---

# Logic Synthesis

## What is `.lib`?
- A `.lib` file is a **standard cell library**.  
- It contains a **collection of logical modules** (basic gates like AND, OR, NOT, etc.).  
- Each gate can have different **flavours** (e.g., 2-input, 3-input, or versions like slow, medium, fast).

---

## Why different flavours?
- Circuits must run at different **clock cycles**.  
- The constraint is: Tclk > TclkA + Tcombinational + TclkB
- Delays in **combinational logic** between sequential elements are important:  
- They ensure the present clock values are held long enough to pass to the next sequential circuit.  
- Hence **slow, medium, and fast flavours** are needed.

---

## Technology reasons
- Delay depends on **capacitor charging and discharging speeds**.  
- **Wide transistors**:  
- Faster  
- But occupy more **area** and consume more **power** (due to higher current handling).  
- **Narrow transistors**:  
- Slower  
- But require less **area** and consume less **power**.

---

## Constraints
- Provide **guidelines** to synthesis tools for selecting which cell flavour (slow, medium, fast) to use in different parts of the circuit.

---

## Netlist
- A **hardware representation** of your RTL code.  
- Generated after logic synthesis, mapping your design to actual gates from the `.lib`.
