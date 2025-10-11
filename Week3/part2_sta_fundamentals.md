# Part 2 - STA Fundamentals

## Static Timing Analysis

### Identification of timing path:
- **Start point:** flop clk / input pin  
- **Stop point:** flop d / output pin  
- **Arrival time:** critical worst time taken to reach stop point from start point.  
- **Required time:** expected time (interval) for the arrival - [expected time min, expected time max]  
- **Slack:** Difference between required time and arrival time  

### Min slack (Setup slack or setup timing or setup analysis)
arrival - mintime  

### Max slack (Hold slack or hold timing or hold analysis)
maxtime - arrival  

### Types of setup/hold analysis
- reg2reg  
- in2reg  
- reg2out  
- in2out  

---

### Clock gating
enabling clk to pass or not  

### Recovery/removal
asynchronous reset through comb or seq circuit timing analysis  

### Data to data
Checks involving timing paths asynchronous gating of reset with datapath (previous flop) with another clock  

### Latch (time borrow / time given)
when clock passed to latch from flop it takes borrow of time for level triggering or vice versa  

---

### Slew / Transition analysis
levels of transitions (signal degradation also effect)
- Data (max/min)
- Clock (max/min)

---

### Load Analysis
- Fanout (max/min)  
- Capacitance  

---

## Clock Analysis
- **Skew:** latency of clock arrival  
- **Pulse width:** how it propagates  

---

## Timing Graphs
Converts circuits to timing graphs: represents delays like Cell delays, wire delays, signal arrival time at inputs  

- **Actual Arrival time:** time at any node the latest transition occurred after the first clock rise edge.  
  Computed from the flop output through upstream inputs. Highest best principle while computation clashes.  

- **Required Arrival time:** time at any node the latest transition expected with the clock cycle.  
  Computed from flop input through downstream outputs. Lowest best principle  

- **Slack (S) = RAT - AAT** at every node  

If negative slack can be removed at any node (i.e., reducing node delay), we can remove overall negative slack  

---

## GBA - PBA
- **Graph based analysis:** worst path analysis  
- **Path based analysis:** actual path taken in silicon  

---

## Detailed Timing Analysis
where nodes are replaced with their pins  

---

## Delays
- **Clock to Q delay**  
- **Skew:** difference between clock arrivals  

When flops made from transistor latches, delay = (inverter + transistor + inverter) (twice for master slave)  

Master slave setup time (for input d to be stable) = 3 inverter delay + one transmission gate delay  
Clock to Q delay = 1 tr + 1 inv  
Hold time = 0  

---

## Jitter
(using eye diagram — the one used in counters to show count — ideal)  

The multiple delay in propagation of clock to another side of chip, or non ideal voltage levels of clock.  
Making the eye diagram so clumsy  

---

## Noise Margin
amount of distortion allowed in voltage levels  

---

## Jitter
temporary variation of clock periods. Thus modelled using parameter called **Uncertainty**  
- Hold uncertainty  
- Setup uncertainty  

Thus **slack = data rt - data at**

---

## Graphical to Textual Representation
- b1/a → net delay  
- b1/y → cell delay  

---

## Sources of Variation
- **Etching:** Everything like shapes, sizes of silicon (various types PMOS, CMOS, poly Si, Si dioxide all depend on etching).  
  Distortions occur due to non ideal fabrication of silicon blocks  
- **Oxide Thickness**  

---

## Relation between resistance, drain current and delay
**OCV (On Chip Variation)** derates used to specify the variation  
