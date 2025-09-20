# rajesh_vsd_tapeout_program
This repository is for full documentation of my journey through VSD ( open to innovate) RISC-V Reference SoC tapeout program. I also believe in documentation along with learning is very powerful tool.  
## Day 0 - Tools Installation  
### Yosys  
Yosys (Yosys Open SYnthesis Suite) is a powerful, free, and open-source framework for hardware synthesis i.e., converting your HDL version of hardware into Gate-level structure.  
Installation steps/commands:  
```
$ sudo apt-get update
$ git clone https://github.com/YosysHQ/yosys.git
$ cd yosys 
$ sudo apt install make (If make is not installed please install it) 
$ sudo apt-get install build-essential clang bison flex \
    libreadline-dev gawk tcl-dev libffi-dev git \
    graphviz xdot pkg-config python3 libboost-system-dev \
    libboost-python-dev libboost-filesystem-dev zlib1g-dev
$ make config-gcc
$ git submodule update --init
$ make 
$ sudo make install
```
<picture><img width="891" height="688" alt="image" src="https://github.com/user-attachments/assets/562bd37e-4078-453c-9ee0-85493d32a265" />
</picture>
Note: git submodule update --init had been added based on the suggestion of terminal as the specific directory exists but is not correctly initialised as a Git submodule in the repository clone. Hence initialised as submodule with this command.  
### Iverilog
Iverilog ( Icarus Verilog) is a popular, free, and open-source Verilog HDL compiler and simulator. It is used to write Verilog code and either compile it for simulation or generate a netlist for synthesis.
Installation Steps/ Commands:
```
$ sudo apt-get update
$ sudo apt-get install iverilog
```
<picture><img width="891" height="282" alt="image" src="https://github.com/user-attachments/assets/0c55531a-3db4-4562-af46-e816a4bbf2ac" />
</picture>
### GTKWave
GTKWave is a free, open-source, and cross-platform waveform viewer used to debug simulations. It is helpful to check does the Design (in HDL) behaves as intended through simulations
Installation Steps/ Commands:
```
$ sudo apt-get update
$ sudo apt install gtkwave
```
<picture><img width="891" height="154" alt="image" src="https://github.com/user-attachments/assets/f78e3acb-168c-4c39-9460-891ccadeac5f" />

</picture>
<picture><img width="868" height="536" alt="image" src="https://github.com/user-attachments/assets/613e5cbb-0e16-47d4-81b2-407b81045f0a" />
</picture>
