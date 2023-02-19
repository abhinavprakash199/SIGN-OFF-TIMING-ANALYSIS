# SIGN-OFF-TIMING-ANALYSIS
This repository contains the whole summary of hands on done by Abhinav Prakash (IS22MTECH14002) during the workshop SIGN-OFF-TIMING-ANALYSIS-using-OpenLane-SKY130 organised by VSD. The full STA flow was implemented using OpenLANE with SKY130nm PDK.

<p align="center">
    <img src="https://user-images.githubusercontent.com/120498080/219842863-80a8bcc6-60cc-4f75-a2ec-f874641fd9bf.png">


* [Day 1](#day-1)
    + [STA Defination](#STA-Defination)
    + [Timing Paths](#Timing-Paths)
    + [Setup & Hold Checks](#Setup-&-Hold-Checks)
    + [Slack Calculation](#Slack-Calculation)
    + [SDC File](#SDC-File)
    + [Clock](#Clock)
    + [Port Delays and Boundary Constrains](#Port-Delays-and-Boundary-Constrains)
    
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
![Screenshot (2402)](https://user-images.githubusercontent.com/120498080/219848964-cef2685a-3754-4076-b282-ab3ba525c545.png)
    
#### Data Requierd Time 
![Screenshot (2407)](https://user-images.githubusercontent.com/120498080/219848972-360c631a-ab9f-4d82-9176-b035e8da5b64.png)

#### Slack
![Screenshot (2404)](https://user-images.githubusercontent.com/120498080/219848973-e921d07f-542d-4b49-bd3b-24619f7aef8b.png)

### SDC File
- A standard file format, Synopsys Design Constraint (SDC), is used to specify timing and other design constraints. The constraints are specified as tcl commands.
- SDC commands are the set of commands which are provided as an input to STA Tool to do timing analysis
- It contain several constrains specifications like 
#### Contraints for Timing
![Screenshot (2408)](https://user-images.githubusercontent.com/120498080/219848815-6d82d719-e80a-4aa5-9c4c-fdc133559229.png)

#### Contraints for Area and Power
![Screenshot (2414)](https://user-images.githubusercontent.com/120498080/219848828-80de1fbf-7999-46af-986e-09e839d67b35.png)

#### Contraints for Design Rule
![Screenshot (2410)](https://user-images.githubusercontent.com/120498080/219848854-a4d123a6-210f-4fc0-86e7-2154d6144239.png)

#### Contraints for Interfaces
![Screenshot (2412)](https://user-images.githubusercontent.com/120498080/219848899-6518b29f-9f24-41d5-8793-ac30e6aa9dca.png)

#### Contraints for Modes
![Screenshot (2411)](https://user-images.githubusercontent.com/120498080/219848917-4902e780-0971-479d-8d51-48988bd3bda9.png)

#### Exceptions to Design Contraints
![Screenshot (2413)](https://user-images.githubusercontent.com/120498080/219848938-486e53a9-51ab-465c-b6bd-d47f1200c704.png)

### Clock
We can define a **clock** signal as the one which synchronizes the state transitions by keeping all the registers/state elements in synchronization. In common terminology, a clock signal is a signal that is used to trigger sequential devices.
    
#### Command to create clock
![Screenshot (2416)](https://user-images.githubusercontent.com/120498080/219851224-c1b71970-141a-4dbe-b8e7-b48f694ea26b.png)
![Screenshot (2420)](https://user-images.githubusercontent.com/120498080/219851373-b29851f1-e365-43f1-9d4c-d77511f935e3.png)
![Screenshot (2421)](https://user-images.githubusercontent.com/120498080/219851483-160a5832-ad29-49f1-877f-916f329a8d1e.png)

**Generated Clocks** are the clocks which are created inside the design which is a divided or multimplied version of a parrent clock 
#### Command to create generated clock
![Screenshot (2423)](https://user-images.githubusercontent.com/120498080/219852273-062713a2-a9fb-4845-974b-8740a1703c68.png)
![Screenshot (2425)](https://user-images.githubusercontent.com/120498080/219852280-b26269fa-b311-49d2-8025-9739beeaee89.png)
![Screenshot (2426)](https://user-images.githubusercontent.com/120498080/219852607-92a789b4-5bca-4c99-b088-b8ec7c144b7e.png)
![Screenshot (2427)](https://user-images.githubusercontent.com/120498080/219853000-51a156eb-7a3f-4979-b345-95d1772b8fe9.png)
    

### Port Delays and Boundary Constrains
- Port Delays can be specified by the following command
```
   set_input_delay
   set_output delay
```
    
- Set Boundary Constrains
    
![Screenshot (2429)](https://user-images.githubusercontent.com/120498080/219943784-a2828d2b-5d5d-41d6-bb80-f1bfeecd6e96.png)
![Screenshot (2430)](https://user-images.githubusercontent.com/120498080/219943792-7df05523-9188-449f-bcb1-067eadf4f34f.png)
![Screenshot (2431)](https://user-images.githubusercontent.com/120498080/219943801-bd0b93e3-4dd2-4223-af7e-50103e848899.png)


  
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 git clone https://github.com/vikkisachdeva/openSTA_sta_workshop
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 

## Appendix
---
    
## References
---
- https://www.synopsys.com/glossary/what-is-static-timing-analysis.html
- https://github.com/Anmol-wq/VSD-IAT-Sign-off-Timing-Analysis---Basics-to-Advanced

## Acknowledgement
---
Finally, I would like to express my sincere gratitude to [Kunal Ghosh](https://www.linkedin.com/in/kunal-ghosh-vlsisystemdesign-com-28084836/){Co-founder of VLSI System Design (VSD) Corp. Pvt. Ltd.} and [Nickson Jose, VLSI Engineer](https://www.linkedin.com/in/nickson-jose/) {ASoC Physical Design Engineer} for tremendous assistance in planning and presenting this workshop on Advanced-Physical-Design-using-OpenLane-SKY130. The workshop was excellent and well designed, this workshop taught me a lot of new things about the design of GDSII file from RTL using OpenLANE, open source tool.   

## Inquiries
- Connect with me at [LinkedIn](https://www.linkedin.com/public-profile/settings?trk=d_flagship3_profile_self_view_public_profile)

