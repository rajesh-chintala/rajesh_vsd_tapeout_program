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
```
read_verilog src/module/vsdbabysoc.v
read_verilog -I src/include/ output/compiled_tlv/rvmyth.v
read_verilog -I src/include/ src/module/clk_gate.v
```  
<img width="1295" height="556" alt="image" src="https://github.com/user-attachments/assets/e626dc8f-70f2-4954-ac6f-d39df1c90a40" />

Step 3: read the liberty files  
```
read_liberty -lib src/lib/avsddac.lib 
read_liberty -lib src/lib/avsdpll.lib 
read_liberty -lib src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```  
<img width="1295" height="352" alt="image" src="https://github.com/user-attachments/assets/3c03d041-c0af-4137-9351-c283533c8261" />  

Step 4: Set the vsdbabysoc as top module.  
```
synth -top vsdbabysoc
```  
you will get output like this:
```
10. Executing SYNTH pass.

10.1. Executing HIERARCHY pass (managing design hierarchy).

10.1.1. Analyzing design hierarchy..
Top module:  \vsdbabysoc
Used module:     \rvmyth
Used module:         \clk_gate

10.1.2. Analyzing design hierarchy..
Top module:  \vsdbabysoc
Used module:     \rvmyth
Used module:         \clk_gate
Removed 0 unused modules.


10.25. Printing statistics.

=== clk_gate ===

        +----------Local Count, excluding submodules.
        | 
        5 wires
        5 wire bits
        5 public wires
        5 public wire bits
        5 ports
        5 port bits

=== rvmyth ===

        +----------Local Count, excluding submodules.
        | 
     3920 wires
     6388 wire bits
      269 public wires
     2722 public wire bits
        3 ports
       12 port bits
     5130 cells
     1316   $_ANDNOT_
      301   $_AND_
      239   $_DFF_P_
      513   $_MUX_
       44   $_NAND_
       45   $_NOR_
       51   $_NOT_
       84   $_ORNOT_
     1318   $_OR_
      962   $_SDFFE_PP0P_
       64   $_SDFFE_PP1P_
        8   $_SDFF_PP0_
       57   $_XNOR_
      128   $_XOR_
        7 submodules
        7   clk_gate

=== vsdbabysoc ===

        +----------Local Count, excluding submodules.
        | 
        9 wires
       18 wire bits
        9 public wires
       18 public wire bits
        7 ports
        7 port bits
        2 cells
        1   avsddac
        1   avsdpll
        1 submodules
        1   rvmyth

=== design hierarchy ===

        +----------Count including submodules.
        | 
     5132 vsdbabysoc
     5130 rvmyth

        +----------Count including submodules.
        | 
     3964 wires
     6441 wire bits
      313 public wires
     2775 public wire bits
       45 ports
       54 port bits
        - memories
        - memory bits
        - processes
     5132 cells
     1316   $_ANDNOT_
      301   $_AND_
      239   $_DFF_P_
      513   $_MUX_
       44   $_NAND_
       45   $_NOR_
       51   $_NOT_
       84   $_ORNOT_
     1318   $_OR_
      962   $_SDFFE_PP0P_
       64   $_SDFFE_PP1P_
        8   $_SDFF_PP0_
       57   $_XNOR_
      128   $_XOR_
        1   avsddac
        1   avsdpll
        1 submodules
        1   rvmyth

10.26. Executing CHECK pass (checking for obvious problems).
Checking module clk_gate...
Checking module rvmyth...
Checking module vsdbabysoc...
Found and reported 0 problems.
```
Step 5: Map D Flip-Flops to Standard Cells
```
dfflibmap -liberty src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```  
<img width="1295" height="556" alt="image" src="https://github.com/user-attachments/assets/75590a5b-72cd-4bf3-a2c7-1c299b2e0a83" />  

Step 6: Perform optimization and technology mapping
<img width="1295" height="556" alt="image" src="https://github.com/user-attachments/assets/d4b62fdf-ec1f-43d6-b2a8-f1b599bbffe7" />  
<img width="1295" height="556" alt="image" src="https://github.com/user-attachments/assets/35c1ff6b-c6d4-4a43-abf2-1594b3418a29" />  

Step 7:  Perform Final Clean-Up and Renaming  
```
flatten
setundef -zero
clean -purge
rename -enumerate
```  
then you can see outputs as  
```

18. Executing FLATTEN pass (flatten design).
Deleting now unused module clk_gate.
Deleting now unused module rvmyth.
<suppressed ~8 debug messages>

19. Executing SETUNDEF pass (replace undef values with defined constants).
Removed 393 unused cells and 7548 unused wires.
```

Step 8: Check Statistics  
`stat`   
you can see output as  
<img width="1295" height="699" alt="image" src="https://github.com/user-attachments/assets/deeb69b5-3a77-4798-9379-e3507df8d03f" />  
<img width="1295" height="699" alt="image" src="https://github.com/user-attachments/assets/8f5094d9-71c1-4465-8b9b-81f3c96f0604" />  
<img width="1307" height="345" alt="image" src="https://github.com/user-attachments/assets/6279935b-69ab-4c35-86d9-650af2703ffa" />  

Step 9: Write the synthesized netlist  
`write_verilog -noattr output/post_synth_sim/vsdbabysoc.synth.v `  
<img width="1307" height="192" alt="image" src="https://github.com/user-attachments/assets/11321533-7811-4e43-a962-34f719237765" />  

#### Post synthesis simulation and waveforms

---

Step 1: Compile the testbench  
```
iverilog -o output/post_synth_sim/post_synth_sim.out -DPOST_SYNTH_SIM -DFUNCTIONAL -DUNIT_DELAY=#1 -I src/include -I src/module src/module/testbench.v
```

Step2: Run the simulation
```
cd output/post_synth_sim
./post_synth_sim.out
```

Step 3: Run the waveform
`gtkwave post_synth_sim.vsd`    
<img width="1307" height="298" alt="image" src="https://github.com/user-attachments/assets/43504daf-e408-4e1f-8b88-e9f6f5f3ed6e" />  
pll output waveform:  
<img width="1308" height="729" alt="image" src="https://github.com/user-attachments/assets/f33317df-3aaf-4f43-bb60-e7221f531048" />  
here there is no separate core block. but we can observe the input and output behaviour in the overall module itself as follows:  
<img width="1308" height="729" alt="image" src="https://github.com/user-attachments/assets/301ca9d7-5040-4114-9399-e1f5c79240bf" />  
the dac waveforms are also same as functional simulation  
<img width="1308" height="729" alt="image" src="https://github.com/user-attachments/assets/6930c265-5b7c-4d01-a826-5c4f056824cd" />  
Then the final top module simulations will result as  
<img width="1308" height="729" alt="image" src="https://github.com/user-attachments/assets/0eb772cf-6f18-4512-9092-fc5847d72a73" />  
Here from all these waveforms where post-synthesis simulations (GLS) are same as pre synthesis simulation (functional simulation). Thus it was clear that our BabySoC behaves as intended even after the synthesis where the hierarchial/ modular circuit become mapped to corresponding gate level nets.
