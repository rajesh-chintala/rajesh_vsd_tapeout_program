# Day 5: Optimization in Synthesis

## If-Else Statements

Conditional execution in behavioral modelling.

### Syntax
```
if (condition) begin
  // Code block executed if condition is true
end else begin
  // Code block executed if condition is false
end
```
Conditions are evaluated to true or false.
begin ... end can be used to group multiple statements.
Omit if only one statement is present.
Also, the else part is optional.

---

## Inferred Latches in Verilog

An inferred latch is a storage element (like a D-latch) created by the synthesis tool when a combinational procedural block fails to assign a value to a signal (reg) under all possible input conditions.
Since latches are level sensitive, they bring a lot of difficulties.

To avoid these inferences, use a default case.

---

## For Loops in Verilog

Used to repeat a process for a defined number of times or iterate over indices.

### Syntax
```
for (initialization; condition; increment) begin
  // Statements to execute
end
```
Synthesizable only if the number of iterations is fixed at compile time.

---

## Generate Blocks

The generate block is the primary mechanism for parameterized instantiation and replication of hardware modules or structural logic (like gates and wires) at compile time.

### genvar

Used exclusively inside generate blocks to declare loop variables.
These are not accessible outside the block and are strictly for compile-time generation.

### Flexibility

Allows the creation of flexible designs like adders, register files, and data paths where the width or number of stages can be easily controlled by a single parameter.

### Example
```
genvar i;
generate
  for (i = 0; i < 4; i = i + 1) begin : gen_loop
    and_gate and_inst (.a(in[i]), .b(in[i+1]), .y(out[i]));
  end
endgenerate
```
