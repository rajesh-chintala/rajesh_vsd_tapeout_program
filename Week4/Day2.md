# Day2 - Threshold Voltage Extraction & Velocity Saturation
## Short channel devices  
Generally MOSFETs operates in 2 regions - Linear Region, Saturation Region. But when the channel length is not enough to satisfy the critical electric field value, then it causes velocity to saturate. This is known as velocity saturation region. To see the Velocity Saturation effect, Model the MOSFECT with smaller widtha nd length.  
```
*Model Description
.param temp=27

*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

*Netlist Description

XM1 Vdd n1 0 0 sky130_fd_pr__nfet_01v8 w=0.39 l=0.15
R1 n1 in 55

Vdd vdd 0 1.8V
Vin in 0 1.8V

*simulation commands
.op
.dc Vdd 0 1.8 0.1 Vin 0 1.8 0.2
.control
run
display
setplot dc1
.endc
.end
```  
Run the spice simulation as follows:  
`ngspice day2_nfet_idvds_L015_W039.spice`  
You can see the following output:  
<img width="1306" height="730" alt="Screenshot from 2025-10-18 15-26-08" src="https://github.com/user-attachments/assets/09325975-afd3-4fc8-9c79-17c27b8dc516" />
<img width="1306" height="295" alt="Screenshot from 2025-10-18 15-26-202" src="https://github.com/user-attachments/assets/923bf22c-2ee4-430f-9df3-ca70a9ce1970" />
Now type `plot -vdd#branch`. Which produces plot as below.
 <img width="1306" height="730" alt="Screenshot from 2025-10-18 15-25-07" src="https://github.com/user-attachments/assets/23432a2c-141a-46be-9b10-d7ae3c582b8b" />  
This is the MOSFET with width = 0.39nm and length = 0.15nm.
Now compare this with the plots from day1 of MOSFET having higher channel width and length.  
<img width="1302" height="743" alt="Screenshot from 2025-10-18 12-22-24" src="https://github.com/user-attachments/assets/8e5f0b33-7e84-4dd0-9830-8055dec45244" />  
 MOSFET with enough width = 5nm and length = 2nm  
This short length caused the Linear dependence in saturation region instead of Quadratic Dependence. ( As from the id equation). Also we can observe that the peak current decreased significantly.

## Threshold Voltage extraction
To calculate the Threshold Voltage (Vt). Run the Id vs Vgs plot as follows:
change the simulation commands as:  
```
*simulation commands
.op
.dc Vin 0 1.8 0.1
```
Now run this spice netlist as  `ngspice day2_nfet_idvgs_L015_W039.spice`  
<img width="1306" height="712" alt="Screenshot from 2025-10-18 15-26-59" src="https://github.com/user-attachments/assets/31854575-3f6b-46ce-8f71-71b487809236" />
<img width="1297" height="190" alt="Screenshot from 2025-10-18 15-27-27" src="https://github.com/user-attachments/assets/59ac4e06-d66b-463b-8c29-ffbd0e618353" />
Now plot using `plot -vdd#branch`, output as follows:
<img width="1302" height="741" alt="Screenshot from 2025-10-18 15-27-45" src="https://github.com/user-attachments/assets/33de51e3-8ba6-4afc-bdbc-cfb9eeb39b4a" />
The plot shows a linear curve of Id vs Vgs curve due to the velocity saturation effect. Now extrapolate the curve onto x-axis, This will be your Threshold voltage which will be around 0.7V
