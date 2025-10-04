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

## Logic Synthesis

### What is `.lib`?
- A `.lib` file is a **standard cell library**.  
- It contains a **collection of logical modules** (basic gates like AND, OR, NOT, etc.).  
- Each gate can have different **flavours** (e.g., 2-input, 3-input, or versions like slow, medium, fast).

---

### Why different flavours?
- Circuits must run at different **clock cycles**.  
- The constraint is: Tclk > TclkA + Tcombinational + TclkB
- Delays in **combinational logic** between sequential elements are important:  
- They ensure the present clock values are held long enough to pass to the next sequential circuit.  
- Hence **slow, medium, and fast flavours** are needed.

---

### Technology reasons
- Delay depends on **capacitor charging and discharging speeds**.  
- **Wide transistors**:  
- Faster  
- But occupy more **area** and consume more **power** (due to higher current handling).  
- **Narrow transistors**:  
- Slower  
- But require less **area** and consume less **power**.

---

### Constraints
- Provide **guidelines** to synthesis tools for selecting which cell flavour (slow, medium, fast) to use in different parts of the circuit.

---

### Netlist
- A **hardware representation** of your RTL code.  
- Generated after logic synthesis, mapping your design to actual gates from the `.lib`.

---

# Labs
Follow the below steps to simulate and synthesize 2-to-1 mux  
**Step1:** Clone the Workshop repository  
```     
cd VLSI/vsdflow/  
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git  
cd sky130RTLDesignAndSynthesisWorkshop/verilog_files
```

**Step2:** Compile the design and testbench  
  
`iverilog good_mux.v tb_good_mux.v`  
generate vcd files by running  
`./a.out`  
view the waveform  
`gtkwave tb_good_mux.vcd`  
Now you can see the below output  
<img width="1007" height="658" alt="Screenshot from 2025-09-21 14-53-19" src="https://github.com/user-attachments/assets/8d70268e-9931-4be5-9241-c1e74ccb464a" />  


**Step3:** Synthesize using Yosys  
  
run yosys  
`yosys`  
run the library  
`read_liberty -lib ~/VLSI/vsdflow/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib`  
run the verilog code  
`read_verilog ~/VLSI/vsdflow/sky130RTLDesignAndSynthesisWorkshop/verilog-files/good_mux.v`  
synthesize the design  
```
synth -top good_mux
abc -liberty ~/VLSI/vsdflow/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
visualize the gate diagram  
`show`  
  
<img width="591" height="637" alt="Screenshot from 2025-09-21 15-09-42" src="https://github.com/user-attachments/assets/6d3234ca-d5bf-4cce-835d-1957870483bd" />
