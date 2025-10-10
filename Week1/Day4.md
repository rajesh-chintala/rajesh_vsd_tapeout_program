

# Day 4: Gate-Level Simulation (GLS), Blocking vs. Non-Blocking in Verilog, and Synthesis-Simulation Mismatch

## Gate-Level Simulation (GLS)

Gate-Level Simulation is an essential verification step that takes place after RTL code has been synthesized. It confirms that the hardware description is correctly implemented before the costly and time-consuming physical design phase (Place and Route) begins.

### GLS Checks

* **Functional Equivalence**
* **Timing Accuracy:** Uses an SDF (Standard Delay Format) file, which contains the precise delay values for every gate and net, generated during synthesis or place-and-route. Helps avoid setup and hold violations.
* **DFT Validation**
* **X-Propagation Check:** Identifies instances where unknown values might have been optimistically treated by the synthesis tool, but cause unexpected, complex behavior in the gate-level design.

### Types of GLS

* **Zero-Delay GLS:** All gates are assumed to have zero delay.
* **Unit-Delay GLS:** All gates are assigned a uniform delay of ‘1’ time unit.
* **Full-Timing GLS:** Uses the SDF file for exact delays. This is the gold standard for checking timing closure before physical layout.

---

## Synthesis-Simulation Mismatch (RTL-to-Gate Mismatch)

A mismatch occurs when the post-synthesis netlist fails to produce the same results as the pre-synthesis RTL code for the same set of inputs.

### Primary Causes of Mismatch

#### 1. Tool Interpretation Differences

* **Ambiguous Sensitivity Lists:**
  In Verilog, if an always @() block does not include all inputs that can cause a change in the output (e.g., missing an enable signal), the simulator only triggers on the listed inputs.
  However, the synthesizer builds gates that depend on all inputs, creating a mismatch.

* **Missing Else Clauses (Latch Inference):**
  The synthesizer infers a latch (a storage element) to hold the previous value when not all branches are covered in a case or if-else statement.
  The simulator, however, may not exhibit this latching behavior correctly, leading to functional differences.

#### 2. Non-Synthesizable Constructs

These are constructs allowed by simulation languages (Verilog/VHDL) but have no direct or consistent mapping to physical hardware gates. The synthesizer may simply ignore them.
Examples include:

* Behavioral delays (e.g., y <= #5 b;)
* Initial blocks
* System functions like $display, $monitor, or $finish

#### 3. Handling of ‘X’ Values (Unknowns)

Optimistic synthesis “optimizes away” logic based on a known constant, which works if inputs are clean.
However, if the RTL can produce an ‘X’ (unknown) under certain conditions, the gate-level logic might handle it differently (propagate it or treat it as ‘0’/‘1’) compared to the RTL simulator — resulting in mismatching outputs.

---

## Blocking vs. Non-Blocking Assignments in Verilog

### Blocking Statements (=)

Execution of the statements below it is **blocked** until the current statement completes.
Each statement’s RHS (right-hand side) is evaluated first and immediately assigned to the LHS (left-hand side).
**Suitable for:** Combinational Circuits.

### Non-Blocking Statements (<=)

Execution of the statements below it is **not blocked**. The assignment is scheduled to occur later.
At the start of the time step (e.g., on a clock edge), the RHS of all non-blocking statements is evaluated using current signal values.
At the end of the time step, all scheduled LHS updates are applied simultaneously.
**Suitable for:** Sequential Circuits.
