# Week 3: Post-Synthesis GLS & STA Fundamentals

### Part 1: Post-Synthesis Simulation - Gate-Level Simulation (GLS) 🔬

**Gate-Level Simulation (GLS)** is used to verify the functionality of a design after the synthesis process. Since the netlist produced after synthesis changed the specified high-level abstraction into actual gates and connections used to implement the design.

***

#### Key Aspects of GLS for BabySoC:

* **Verification with Timing Information:** GLS is performed using **Standard Delay Format (SDF) files** to ensure timing correctness.
* **Design Validation Post-Synthesis:** Confirms that the design's logical behavior remains correct after mapping it to the gate-level representation.
* **Simulation Tools:** Tools like **Icarus Verilog** or a similar simulator can be used for compiling and running the gate-level netlist.
* **Waveform Analysis:** Waveforms are typically analyzed using **GTKWave**.  

---

### Step by Step Guide for GLS:
Step 1: Launch the yosys shell
`yosys`
<img width="1295" height="295" alt="image" src="https://github.com/user-attachments/assets/5698825d-41e6-42f0-803d-c5ce36295b98" />  

Step 2: read the verilog files
