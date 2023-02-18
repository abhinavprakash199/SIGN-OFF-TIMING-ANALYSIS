# SIGN-OFF-TIMING-ANALYSIS
This repository contains the whole summary of hands on done by Abhinav Prakash (IS22MTECH14002) during the workshop SIGN-OFF-TIMING-ANALYSIS-using-OpenLane-SKY130 organised by VSD. The full STA flow was implemented using OpenLANE with SKY130nm PDK.

![Screenshot (2395)](https://user-images.githubusercontent.com/120498080/219842863-80a8bcc6-60cc-4f75-a2ec-f874641fd9bf.png)


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
![Screenshot (2396)](https://user-images.githubusercontent.com/120498080/219844063-4006e064-51f4-4425-ba65-e2c9842578b8.png)

#### Inputs to an STA 
- Gate level netlist which has a design representation
- SDC or Input Constrains file
- Logic Libraries which contain informattion about our standard cells, gates and their delays, transition time

### Timing Paths
When performing timing analysis, STA first breaks down the design into smaller portions called **timing paths**. 
STA breaks the path at 
- Ports 
- Sequential Elements
![Screenshot (2397)](https://user-images.githubusercontent.com/120498080/219843999-a4e6ad2a-e673-4d51-9ec3-b143904eb35a.png)


Each timing path consists of the following elements:

- **Startpoint** - The start of a timing path where data is launched by a clock edge or where the data must be available at a specific time. Every startpoint must be either an input port or a register clock pin.
- **Combinational logic network** - Elements that have no memory or internal state. Combinational logic can contain AND, OR, XOR, and inverter elements, but cannot contain flip-flops, latches, registers, or RAM.
- **Endpoint** - The end of a timing path where data is captured by a clock edge or where the data must be available at a specific time. Every endpoint must be either a register data input pin or an output port.
![Screenshot (2399)](https://user-images.githubusercontent.com/120498080/219844485-17755126-109f-413f-8c49-58c4b587bd9b.png)

A combinational logic cloud might contain multiple paths, as shown in the following figure. STA uses the longest path to calculate a maximum delay and the shortest path to calculate a minimum delay.
![Screenshot (2400)](https://user-images.githubusercontent.com/120498080/219844793-66a62bf4-9dd2-437a-893a-a80b8b9f6016.png)

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
![Screenshot (2402)](https://user-images.githubusercontent.com/120498080/219847626-790136dd-0486-44b3-b924-8d6e30f6f2b3.png)
#### Data Requierd Time 
![Screenshot (2407)](https://user-images.githubusercontent.com/120498080/219847653-139844fe-c626-4ff2-a30e-8d784cdfb36c.png)
#### Slack
![Screenshot (2404)](https://user-images.githubusercontent.com/120498080/219847660-dcf60d68-4815-42e2-b27b-c49cbe408e61.png)


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

