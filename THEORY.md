* [Day 1](#day-1)
    + [STA Defination](#STA-Defination)
    + [Timing Paths](#Timing-Paths)
    + [Setup & Hold Checks](#Setup-&-Hold-Checks)
    + [Slack Calculation](#Slack-Calculation)
    + [SDC File](#SDC-File)
    + [Clock](#Clock)
    + [Port Delays and Boundary Constrains](#Port-Delays-and-Boundary-Constrains)

* [Day 2](#day-2)
    + [Other Timing Checks](#Other-Timing-Checks)
    + [Design Rule Checks](#Design-Rule-Checks)
    + [Latch Timing](#Latch-Timing)
    + [STA Text Report](#STA-Text-Report)

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


## Day 2
---    
### Other Timing Checks
Apart from Hold and Setup checks(which happens in data pins with respect to data pins) STA also dose other types of check like 
- Clock Gating Checks(done on clock enable pin with respect to the clock pin)
- Async Pin Checks(checks when reset pin, set pin, clear pin can be inserted with respect to clock) 
- Data to Data Checks(identify may skew between 2 pins)
    
![Screenshot (2432)](https://user-images.githubusercontent.com/120498080/220008152-e9e18ba2-8a53-46d2-ba26-ba5f8b6d62c9.png)
### Design Rule Checks
Design Rule Checks specifies about
1. Slew/Transition Analysis
    
![Screenshot (2433)](https://user-images.githubusercontent.com/120498080/220009643-a2cb650b-0447-4aae-9da2-fff4fbc33ed5.png)
  
2. Load Analysis
- We can specify minimum and maximum capacitance on ports and nets.
- Also specify the maximum fanout load on ports and output pins.
    
3. Clock Skew Analysis
- It is difference in delay of the clock at different points.
- It is basically the delay between the launch clock and capture clock.
- Skew is said to be positive when caputre clock has more delay.
    
![Screenshot (2435)](https://user-images.githubusercontent.com/120498080/220009675-1187a5b1-ab0f-4735-8176-62f54bb8f021.png)
    
4. Pulse Width Checks
- As clock travels through the path the clock pulse can change(can happen when rise delay and fall delay are not same) called as Shrinked Clock.
- If this shrinked clock is below the certain value then pulse width violation happens.
    
![Screenshot (2436)](https://user-images.githubusercontent.com/120498080/220012119-0e931c35-392c-429e-b1fa-e73cffd47534.png)

### Latch Timing
**Latch can start sampling data from the rising edge(or falling edge) itself and continue sampling till the respective falling edge (or rising edge)** because latches are level sensitive whereas **Flipflop can only sample the data "at" Rising edge or Negative edge** because flipflops are edge sensitive. Both holds the data when they are disable (Latch disable at level and Flipflop disable just after triggering level).
    
Generally designers prefer flip flops over latches because of this edge-triggered property, which makes their life easy to do analysis and interpretation the design. 
    
Latch-based designs are preferred in case of clock frequency in GHz (in high-speed designs). In flip-flop-based high-speed designs, maintaining clock skew is a problem, but latches ease this problem.
    
#### Time Borrowing
Time borrowing is the property of a latch by virtue of which a path ending at a latch can borrow time from the next path in pipeline such that the overall time of the two paths remains the same. The time borrowed by the latch from next stage in pipeline is, then, subtracted from the next path's time.
    
![Screenshot (2437)](https://user-images.githubusercontent.com/120498080/220060699-0882e529-c0a7-48fe-ae9d-930671ca6b97.png)

### STA Text Report
When STA tool dose analysis and reports setup and hold timing, its first converts that logic into nodes and cells and archs.
  
![Screenshot (2438)](https://user-images.githubusercontent.com/120498080/220064935-353bce95-62b9-43e4-81ae-871a6a9e0994.png)

#### Sample STA Text Report
![Screenshot (2441)](https://user-images.githubusercontent.com/120498080/220065230-b7350659-e6a7-4edc-92ac-ae32057a372f.png)    
