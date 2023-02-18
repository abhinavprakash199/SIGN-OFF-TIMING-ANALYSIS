# SIGN-OFF-TIMING-ANALYSIS
This repository contains the whole summary of hands on done by Abhinav Prakash (IS22MTECH14002) during the workshop SIGN-OFF-TIMING-ANALYSIS-using-OpenLane-SKY130 organised by VSD. The full STA flow was implemented using OpenLANE with SKY130nm PDK.

<p align="center">
    <img src="https://user-images.githubusercontent.com/120498080/219842863-80a8bcc6-60cc-4f75-a2ec-f874641fd9bf.png">


* [Day 1](#day-1)
    + [STA Defination](#STA-Defination)
    + [Timing Paths](#Timing-Paths)
    + [Setup & Hold Checks](#Setup-&-Hold-Checks)
    
* [Appendix](#Appendix)
* [References](#references)
* [Acknowledgement](#acknowledgement)
* [Inquiries](#inquiries)

## Day 1
---
### STA Defination
Static timing analysis (STA) is a method of validating the timing performance of a design by checking all possible paths for timing violations. STA breaks a design down into timing paths, calculates the signal propagation delay along each path, and checks for violations of timing constraints inside the design and at the input/output interface.
    
<p align="center">
    <img src="https://user-images.githubusercontent.com/120498080/219847750-36e15d0e-38ff-42e0-a105-7b4234270d7b.png">

#### Inputs to an STA 
- Gate level netlist which has a design representation
- SDC or Input Constrains file
- Logic Libraries which contain informattion about our standard cells, gates and their delays, transition time

### Timing Paths
When performing timing analysis, STA first breaks down the design into smaller portions called **timing paths**. 
STA breaks the path at 
- Ports 
- Sequential Elements
<p align="center">
    <img src="https://user-images.githubusercontent.com/120498080/219847761-3572ebee-a802-4e65-bcce-ea63ed887ce7.png">

Each timing path consists of the following elements:

- **Startpoint** - The start of a timing path where data is launched by a clock edge or where the data must be available at a specific time. Every startpoint must be either an input port or a register clock pin.
- **Combinational logic network** - Elements that have no memory or internal state. Combinational logic can contain AND, OR, XOR, and inverter elements, but cannot contain flip-flops, latches, registers, or RAM.
- **Endpoint** - The end of a timing path where data is captured by a clock edge or where the data must be available at a specific time. Every endpoint must be either a register data input pin or an output port.
<p align="center">
    <img src="https://user-images.githubusercontent.com/120498080/219847775-09ca2042-0b46-4c1f-b6ff-918e7fb9a7b2.png">

A combinational logic cloud might contain multiple paths, as shown in the following figure. STA uses the longest path to calculate a maximum delay and the shortest path to calculate a minimum delay.
<p align="center">
    <img src="https://user-images.githubusercontent.com/120498080/219847787-7cf1cadd-e262-48d2-a757-90ff3075a1f7.png">

### Setup & Hold Checks
- A **setup check** specifies how much time is necessary for data to be available at the input of a sequential device before the clock edge that captures the data in the device. This check enforces a **maximum delay** on the data path relative to the clock edge. 
- A **hold check** specifies how much time is necessary for data to be stable at the input of a sequential device after the clock edge that captures the data in the device. This check enforces a **minimum delay** on the data path relative to the clock edge.
- Setup and Hold time of a flop is dependent on 
    1. Technology node(65nm or 180nm)
    2. or values are available in logic libraries 
 
### Slack Calculation 
```
Slack = Data Requierd Time - Data Arrival Time 
```

#### Data Arrival Time 
<p align="center">
    <img src="https://user-images.githubusercontent.com/120498080/219847626-790136dd-0486-44b3-b924-8d6e30f6f2b3.png">
#### Data Requierd Time 
<p align="center">
    <img src="https://user-images.githubusercontent.com/120498080/219847653-139844fe-c626-4ff2-a30e-8d784cdfb36c.png">
#### Slack
<p align="center">
    <img src="https://user-images.githubusercontent.com/120498080/219847660-dcf60d68-4815-42e2-b27b-c49cbe408e61.png">


## Appendix
---
    
## References
---
- https://www.synopsys.com/glossary/what-is-static-timing-analysis.html

## Acknowledgement
---
Finally, I would like to express my sincere gratitude to [Kunal Ghosh](https://www.linkedin.com/in/kunal-ghosh-vlsisystemdesign-com-28084836/){Co-founder of VLSI System Design (VSD) Corp. Pvt. Ltd.} and [Nickson Jose, VLSI Engineer](https://www.linkedin.com/in/nickson-jose/) {ASoC Physical Design Engineer} for tremendous assistance in planning and presenting this workshop on Advanced-Physical-Design-using-OpenLane-SKY130. The workshop was excellent and well designed, this workshop taught me a lot of new things about the design of GDSII file from RTL using OpenLANE, open source tool.   

## Inquiries
- Connect with me at [LinkedIn](https://www.linkedin.com/public-profile/settings?trk=d_flagship3_profile_self_view_public_profile)

