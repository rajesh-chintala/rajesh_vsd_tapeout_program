# CMOS Inverter: Voltage Transfer Characteristic (VTC)

### MOSFET as a Switch

<img width="381" height="218" alt="Screenshot from 2025-10-18 16-19-15" src="https://github.com/user-attachments/assets/109d288c-079f-4dab-a11d-96ae80d8e543" />  

  |Vgs|>|Vt| makes the switch ON vise versa.
Had infinite OFF resistance.  
  
### CMOS - Complementary MOSFET  
It is combination of NMOS and PMOS with drains both connected, gates shorted with input, sources connected to Voltage sources. As below figure,  
<img width="480" height="435" alt="Screenshot from 2025-10-18 16-17-01" src="https://github.com/user-attachments/assets/e3a4769d-a195-4e2f-af42-905c27e1fbbe" />  
  When Vin high, (equal to Vdd), PMOS turns OFF, NMOS turns on. Then as switch turns on output connected to ground. Thus output zero. input inverted.
When Vin is low, converse happens and output becomes high.
Thus CMOS Transfer characteristics can be plot from the common points of PMOS and NMOS characteristics in terms of Vout and Vin.

### Voltage Transfer Characteristics
Spice  Deck (netlist) for a CMOS Inverter is as follows:
```
*Model Description
.param temp=27

*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

*Netlist Description

XM1 out in vdd vdd sky130_fd_pr__pfet_01v8 w=0.84 l=0.15
XM2 out in 0 0 sky130_fd_pr__nfet_01v8 w=0.36 l=0.15


Cload out 0 50fF

Vdd vdd 0 1.8V
Vin in 0 1.8V

*simulation commands

.op

.dc Vin 0 1.8 0.01

.control
run
setplot dc1
display
.endc

.end
```
Generally, PMOS Channel width is taken greater than the NMOS (here around 2.33 times).  
Now run the spice using the command `ngspice day3_inv_vtc_Wp084_Wn036.spice `  
which results in the following output:
<img width="1303" height="741" alt="Screenshot from 2025-10-18 17-55-32" src="https://github.com/user-attachments/assets/6db5ede3-22e0-4063-a117-c18a595297ef" />
Now plot the graphs using  `plot out vs in`, which give output as follows:
<img width="1303" height="741" alt="Screenshot from 2025-10-18 17-52-02" src="https://github.com/user-attachments/assets/40e17f7e-a362-4b79-a83e-47c030067849" />
Now to find the switching threshold, where the Vin = Vout. Zoom into the plot as follows
<img width="1303" height="741" alt="Screenshot from 2025-10-18 17-54-15" src="https://github.com/user-attachments/assets/00c6c06f-bb69-4034-acb5-002c1f5c5bb6" />
click on the equal point, then you can see the values as 
<img width="1301" height="187" alt="Screenshot from 2025-10-18 17-57-57" src="https://github.com/user-attachments/assets/8df44e35-e82b-4b1b-8b52-d4534c3bc375" />
Thus 0.877 is the Switching Threshold.  
Note: If the Widths of both mosfets of equal size, the switching threshold will be half of the voltage range.  

# Transient Behavior: Rise / Fall Delays
To observe the transient behaviour in a CMOS inverter, perform the puslse input as following  
```
Vin in 0 PULSE(0V 1.8V 0 0.1ns 0.1ns 2ns 4ns)

*simulation commands

.tran 1n 10n
```
run the command, `ngspice day3_inv_tran_Wp084_Wn036.spice`  , you will see the following output  
<img width="1301" height="744" alt="image" src="https://github.com/user-attachments/assets/b2a5d17f-6144-48f4-99ff-b8dab4eab282" />  
plot the waveforms using `plot out vs time in`
<img width="1301" height="744" alt="image" src="https://github.com/user-attachments/assets/ff45199d-a552-4ed2-849a-b11ee0555226" />
To caluclate the rise/ fall delays, identify the crossing point of the rising and falling out wave (Vdd/2)i.e, here around 0.9.  
At that point, zoom to the falling edge as follows
<img width="1301" height="744" alt="image" src="https://github.com/user-attachments/assets/12809c58-48a1-4a46-9c2e-27cb25b56eb3" />
click on the corresponding out and in waves, the difference between falling out value and in value at 0.9, those values are as follows:
<img width="1300" height="623" alt="image" src="https://github.com/user-attachments/assets/d01890ff-f123-4822-be9a-718f5cc4598f" />

Thus the Falling delay is 0.338667ns - 0.050667ns = 0.288ns

Now at the same level, zoom to the rising edge as follows:
<img width="1301" height="744" alt="image" src="https://github.com/user-attachments/assets/f7aa57fc-d644-4eff-b983-1b21591d0081" />
click on the corresponding out and in values, to annotate the points
<img width="1300" height="110" alt="image" src="https://github.com/user-attachments/assets/f9c1d572-2c82-4359-bcfe-2ebcf72f2d73" />
thus the rising delays is 2.48133ns - 2.14933ns = 0.332ns

Hence the Rising delay is 0.332ns and falling delay is 0.288ns.
