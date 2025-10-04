# Functional Modelling ( Lab Practical )

---

### Project Directory Structure
```
VSDBabySoC/
├── src/
│   ├── include/
│   │   ├── sandpiper.vh
│   │   └── other header files...
│   ├── module/
│   │   ├── vsdbabysoc.v      # Top-level module integrating all components
│   │   ├── rvmyth.v          # RISC-V core module
│   │   ├── avsdpll.v         # PLL module
│   │   ├── avsddac.v         # DAC module
│   │   └── testbench.v       # Testbench for simulation
└── output/
└── compiled_tlv/         # Holds compiled intermediate files if needed
```

---

### VSDBabySoC Modeling

The **VSDBabySoC** is modeled and simulated using **iverilog** and results are viewed with **gtkwave**.  
Initial inputs trigger the **PLL** to generate the system clock, which drives the **RVMYTH CPU**. The CPU updates its **r17 register** each cycle, and these values are converted by the **DAC** into the final output signal **`OUT`**.  

#### Key Elements
- **PLL:** Generates clock  
- **RVMYTH:** Executes instructions, updates `r17`  
- **DAC:** Converts digital values to `OUT`  
- **Wrapper & Testbench:** Integrate and verify the system

---

## Step by Step Guide for modeling
1. Install neccessary packages
    ```
    sudo apt install make python python3 python3-pip git iverilog gtkwave docker.io

    #if docker doesn't installing on newer ubuntu, try
    sudo apt remove -y containerd.io
    sudo apt autoremove -y
    sudo apt update
    sudo apt install -y docker.io
    sudo apt install -y make python3 python3-pip git iverilog gtkwave docker.io python-is-python3

    sudo chmod 666 /var/run/docker.sock
    cd ~
    pip3 install pyyaml click sandpiper-saas
   ```
2. Clone the repo  `git clone https://github.com/manili/VSDBabySoC.git`
3. make the vcd files
   ```
   cd VSDBabySoC
   make pre_synth_sim`
   ```  
4. See the waveforms  `gtkwave output/pre_synth_sim/pre_synth_sim.vcd`
5. Enable the required signals, output should be as follows:  

---

## OPENLANE
OpenLANE is an automated RTL to GDSII flow based on several components including OpenROAD, Yosys, Magic, Netgen, Fault, SPEF-Extractor and custom methodology scripts for design exploration and optimization.  
Follow this commands for installation of OPENLANE
```
git clone https://github.com/The-OpenROAD-Project/OpenLane.git
cd OpenLane/
make openlane
make pdk
make test
```
