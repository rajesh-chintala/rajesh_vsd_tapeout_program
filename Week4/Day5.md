# Power-Supply and Device Variation Studies
The Power supply won't be fixed value in time. From past to present, we are moving towards the low power circuits. Thsu we expect our Cmos to work in those situations also. Hence let's simulate our CMOS design at various power supply values.
Below is the spice program for the power supply variation
```
*Model Description
.param temp=27


*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt


*Netlist Description


XM1 out in vdd vdd sky130_fd_pr__pfet_01v8 w=1 l=0.15
XM2 out in 0 0 sky130_fd_pr__nfet_01v8 w=0.36 l=0.15


Cload out 0 50fF

Vdd vdd 0 1.8V
Vin in 0 1.8V

.control

let powersupply = 1.8
alter Vdd = powersupply
        let voltagesupplyvariation = 0
        dowhile voltagesupplyvariation < 6
        dc Vin 0 1.8 0.01
        let powersupply = powersupply - 0.2
        alter Vdd = powersupply
        let voltagesupplyvariation = voltagesupplyvariation + 1
      end

plot dc1.out vs in dc2.out vs in dc3.out vs in dc4.out vs in dc5.out vs in dc6.out vs in xlabel "input voltage(V)" yl
abel "output voltage(V)" title "Inveter dc characteristics as a function of supply voltage"

.endc

.end
```  
Here the powersupply variable declared, defined and updated using let keyword. alter keyword is used to assign this variable value to the node at netlist. this variable is looped through 5 different values, producing dc plot at every instant.  
Run the spice program using, `ngspice day5_inv_supplyvariation_Wp1_Wn036.spice`  
<img width="1300" height="736" alt="image" src="https://github.com/user-attachments/assets/5b486f8e-7cd5-43d4-8b3a-f6cabe073262" />
It will produce plot as follows:
<img width="1300" height="736" alt="image" src="https://github.com/user-attachments/assets/d5fd80a0-3d7c-4c5c-bf7f-d5e8444d4e8c" />
