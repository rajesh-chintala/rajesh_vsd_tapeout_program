# Timing Libraries, Synthesis Approaches, and Efficient Flip-Flop Coding

---

## Timing Libraries

The **SKY130 PDK (Process Design Kit)** is an open-source collection of files and documentation that allows engineers to design integrated circuits (ICs) using **SkyWater Technology’s 130nm CMOS process**.

---

### Process Design Kit (PDK)

The PDK contains all the essential data needed to ensure that a circuit design functions correctly when manufactured. It includes:

- **Design Rules** – Geometric constraints for drawing shapes on the chip.  
- **Device Models** – Mathematical models for transistors, resistors, and capacitors that predict their electrical behavior.  
- **Standard Cell Libraries** – Pre-designed, characterized blocks (like logic gates: AND, OR, XOR) used to build digital circuits.  

---

### Decoding the Naming Convention

The naming convention defines the **performance** of standard cells in the library.  
Performance (speed, power consumption, and reliability) depends on three main factors:

#### 1. Process Corner

The **process corner** describes variations in manufacturing that affect transistor speed:

| Corner | Meaning | Description |
|:-------|:--------|:-------------|
| `tt` | typical-typical | Both NMOS and PMOS have typical speeds |
| `ff` | fast-fast | Both types are faster than typical |
| `ss` | slow-slow | Both types are slower than typical |
| `fs` / `sf` | fast-slow / slow-fast | One transistor type is fast, the other is slow |

---

#### 2. Temperature

Specifies the operating condition of the chip:

| Label | Temperature | Description |
|:------|:-------------|:------------|
| `025C` | 25°C | Nominal or room temperature — baseline condition |
| `-40C` | -40°C | Cold corner — used for extreme low-temperature checks |
| `125C` | 125°C | Hot corner — worst-case scenario for speed |

---

#### 3. Core Voltage

Defines the supply voltage used by the chip’s core logic:

| Label | Voltage | Description |
|:------|:---------|:------------|
| `1v80` | 1.80V | Nominal (target) voltage for SKY130 process |
| `1v62` | 1.62V | Low-voltage condition (~10% below nominal) |
| `1v98` | 1.98V | High-voltage condition (~10% above nominal) |

IC designers use this convention to perform **Static Timing Analysis (STA)** to ensure timing reliability across all **PVT** conditions.

---

### Synthesis Approaches

#### Hierarchical Synthesis
Preserves the original module structure, treating each module as a **separate black box**.

**Advantages:**
- Maintains modular hierarchy (tree-like structure).
- Faster synthesis runtime.
- Easier debugging due to clear module boundaries.

**Disadvantages:**
- Limited cross-module optimization.

---

#### Flattened Synthesis
Merges the entire design into a **single netlist**, removing all module boundaries for **global optimization**.

**Advantages:**
- Better optimization across modules.
- Can produce smaller, faster, and more efficient chips.

**Disadvantages:**
- Consumes more memory and runtime.
- Debugging becomes harder (no module boundaries).

---

### Comparison via Yosys Scripts

You can visualize the difference between **hierarchical** and **flattened** synthesis using the following Yosys scripts:

#### synth_flat.ys
```bash
# Flattened Synthesis Script
read_verilog my_design.v
hierarchy -top top_module
proc
flatten
opt_clean
techmap
abc
clean
write_verilog flat_netlist.v
show
```  
output will be as follows  
<img width="1295" height="743" alt="Screenshot from 2025-10-04 11-45-05" src="https://github.com/user-attachments/assets/57d61d89-8dda-4580-853f-48ef8dca6f9e" />

#### synth_hierarchial.ys
```bash
# Hierarchical Synthesis Script
read_verilog my_design.v
hierarchy -top top_module
# Note: The 'flatten' command is omitted to preserve module structure.
# The 'opt' and other passes still run, but cross-module optimizations are limited.
proc
opt_clean
techmap
abc -D     # -D disables coarse-grain flattening in ABC
clean
write_verilog hier_netlist.v
show
```  
output will be as follows  
<img width="608" height="651" alt="Screenshot from 2025-10-04 10-32-24" src="https://github.com/user-attachments/assets/1ab8a0bb-92af-4ab0-b591-fcabd57568d4" />
