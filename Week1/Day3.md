# Day 3: Combinational and Sequential Optimization

---

## Constant Propagation

**Constant Propagation** is a compiler optimization applied during synthesis.  
It analyses the data flow and replaces variables or expressions being constant throughout their scope with their constant value.  
This helps in:

- Logic Simplification  
- Gate Reduction  
- Faster Timing  

---

## State Optimization

**State Optimization** is crucial for designing efficient and reliable **Finite State Machines (FSMs)**.  
Techniques involve:

- **State Reduction:** Minimizes states, thus reducing area and power.  
- **State Encoding:** The representation of states using a specific system. The choice of encoding significantly impacts the size and speed of the combinational logic.  
- **Logic Minimization:** After encoding, synthesis tools further make compact and efficient gate-level implementation.  

---

## Cloning (Logic Duplication)

**Cloning (Logic Duplication)** is a **physical optimization technique** used primarily in the **Physical Design (Place and Route)** stage to solve timing, power, and loading issues.  

When a single logic cell drives a very large fanout (many downstream cells), two problems arise:

1. **Timing Violation:**  
   The delay along the path increases because the driver cell must charge and discharge the high total capacitive load of all the driven nets.

2. **Signal Integrity:**  
   High fanout leads to long interconnect wires, increasing power consumption and potential noise (crosstalk).

Thus, the driver cell is **duplicated (cloned)** *N* times.  
The original set of driven cells is then partitioned and divided among the *N* cloned driver cells.  

This resolves high-fanout nets, reduces skew, and minimizes wire length.

---

## Retiming

**Retiming** is a **sequential optimization technique** that moves registers across combinational logic blocks without altering the overall system function (delay balancing).

### A. Graph Model

The circuit is modeled as a **directed graph** where:

- **Nodes** represent combinational logic blocks.  
- **Edges** represent wires.  
- **Weights** on the edges represent the number of registers (flip-flops) on that wire.  

### B. Register Movement Rules

- **Across a Combinational Block (Backward):**  
  A register on the input edge of a combinational block can be moved to all output edges of that block.

- **Across a Combinational Block (Forward):**  
  Registers on all input edges of a combinational block can be moved to the single output edge.

### C. Optimization Goal

**Goal:** Minimize the clock period (T<sub>clk</sub>).  

By moving registers, the tool ensures that the maximum combinational delay between any two registers is reduced.  
This maximum delay dictates the shortest possible clock period.  

Retiming effectively balances the delay by pushing registers from **logic-sparse paths** into **logic-dense paths**.

---
