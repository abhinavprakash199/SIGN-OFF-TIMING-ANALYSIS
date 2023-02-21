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
    
* [Day 3](#day-3)
    + [Multiple Clocks](#Multiple-Clocks)
    + [Types of Arch](#Types-of-Arch)
    + [Cell Delays](#Cell-Delays)
    + [Clock Skew](#Clock-Skew)
    + [Clock Latency](#Clock-Latency)
    + [Clock Jitter](#Clock-Jitter)
    + [Setup Check](#Setup-Check)
    + [Hold Check](#Hold-Check)
    + [CRPR-Clock Reconvergence Pessimism Removal](#CRPR-Clock-Reconvergence-Pessimism-Removal)

* [Day 4](#day-4)
    + [Crosstalk and Noise](#Crosstalk-and-Noise)
    + [Process Variation](#Process-Variation)
    + [Clock Gating Checks](#Clock-Gating-Checks)
    + [Checks on Async Pins](#Checks-on-Async-Pins)

* [Day 5](#day-5)  
    + [Clock Groups](#Clock-Groups)

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
    
## Day 3
---
### Multiple Clocks
When different types of clock are present(of different frequency) then setup check is calculated or the most restrictive setup is identified by first expanding a clock by a **common base period** and in that whichever is the closest launch and capture which is used find the most restrictive setup. The hold check is dependent upon the setup check.
#### Setup Check
![Screenshot (2453)](https://user-images.githubusercontent.com/120498080/220174671-1e2144bf-a536-416a-85c0-7a4bd32ee4b3.png)
#### Hold Check
There are tro rules for Hold Check
1.
    ![Screenshot (2456)](https://user-images.githubusercontent.com/120498080/220174940-6e48e9b7-f132-4b30-a3fb-717030970964.png)
2. 
    ![Screenshot (2455)](https://user-images.githubusercontent.com/120498080/220174912-32a1e3a2-7a24-4ab8-a7e6-df7fe2230f72.png)

![Screenshot (2454)](https://user-images.githubusercontent.com/120498080/220174999-2b3d1729-af1a-458e-8898-ba61927ce858.png)

### Types of Arch 
#### Timing Arch 
They are of two types 
1. **Cell Arch** - It is the input to output connection in the cell present in logical libraries . These arch will have information about the input-output functionality, rise and fall time, delays, etc. 
2. **Net Arch** - It connects one cell to other cell and there are ways to calculate the information about the cell to cell rise and fall time, delays, etc.

![Screenshot (2457)](https://user-images.githubusercontent.com/120498080/220179768-46cd79a4-1597-4ee4-9647-429956367399.png)

#### Combinational Archs
These archs don't involve any state element or memory device (like combinational circuits).

![Screenshot (2458)](https://user-images.githubusercontent.com/120498080/220180142-59855bda-ffb5-496c-9432-a716be870cfe.png)
 
#### Sequentional Archs
- These are related to clocks like our setup and hold timing. There are setup and hold arch which tells us about the setup and hold time requirement of the library. 
- It has delay arch defined in the logical library.
    
![Screenshot (2459)](https://user-images.githubusercontent.com/120498080/220180141-26fa2fea-406a-40de-8e76-9f14251ddd37.png)

#### Positive Unate Arch
Positive Unate Arch follows the input in same direction. It basically tells us that when input rises the output also rises or when input falls the output also going to fall.
    
![Screenshot (2460)](https://user-images.githubusercontent.com/120498080/220180335-88c2756e-4d47-4d87-82c2-1e88e0d4a625.png)

#### Negative Unate Arch 
Negative Unate Arch follows the input in opposite direction. It basically tells us that when input rises the output falls or when input rises the output also going to fall.
    
![Screenshot (2461)](https://user-images.githubusercontent.com/120498080/220180334-52041390-2ed7-49e1-ab6e-1b5bc5e08da3.png)

#### Non Unate Arch
In Non Unate Arch output cant be predicted the output when input changes.
    
![Screenshot (2463)](https://user-images.githubusercontent.com/120498080/220180418-28b0788b-96fc-4009-992c-a6a443c5d934.png)

 
### Cell Delays 
- Cell Arch have these Cell Delays 
    
![Screenshot (2465)](https://user-images.githubusercontent.com/120498080/220181078-10ab6c17-25f9-42b2-932d-06c5c93c13a5.png)

### Clock Skew
- Clock Skew is the time difference between arrival of the same edge of a clock signal at the Clock pin of the capture flop and launch flop.
- It is basically the delay difference between the two clocks.
    
![Screenshot (2466)](https://user-images.githubusercontent.com/120498080/220183083-b319876a-aa90-4a5a-88a3-0c303cbd7fa3.png)
- So because of the positive skew we got the extra time to meet setup and it makes setup time easer to meet, whereas to meet hold time we need to keep the data stable for more time. 
    
![Screenshot (2467)](https://user-images.githubusercontent.com/120498080/220183110-e4aa164a-f019-4664-9bec-d9922065a86c.png)
- So because of the negative skew we got the less time to meet setup and it makes setup time difficult to meet, whereas to meet hold time we need to keep the data stable for less time. 
 
### Clock Latency
- The time taken by Clock signal to reach from clock source to the clock pin of a particular flip flop is called as Clock latency.
    
![Screenshot (2468)](https://user-images.githubusercontent.com/120498080/220183662-94cbea73-03f0-4a70-ae2b-1df8b2e0e1a4.png)


### Clock Jitter
- Clock jitter is a characteristic of the clock source and the clock signal environment. It can be defined as “deviation of a clock edge from its ideal location.” Clock jitter is typically caused by clock generator circuitry, noise, power supply variations, interference from nearby circuitry etc.
    
![Screenshot (2469)](https://user-images.githubusercontent.com/120498080/220183707-c17713db-3ef3-41fb-9cad-e52ec50015ce.png)
 
### Setup Check
![Screenshot (2471)](https://user-images.githubusercontent.com/120498080/220192551-c8c93b42-4f29-4820-a7e4-f183a3627173.png)
![Screenshot (2473)](https://user-images.githubusercontent.com/120498080/220192560-3d0a3679-87d6-4589-b753-24e05ef6283b.png)
![Screenshot (2477)](https://user-images.githubusercontent.com/120498080/220192613-e7588eea-9342-48f5-856b-d8b4b2c32a6f.png)
 
### Hold Check
![Screenshot (2474)](https://user-images.githubusercontent.com/120498080/220193019-72e6054d-afe9-4514-b16e-cb1423c7ac97.png)
![Screenshot (2476)](https://user-images.githubusercontent.com/120498080/220193051-2bfa198c-6317-495f-977e-a2532b20f1ff.png)
![Screenshot (2478)](https://user-images.githubusercontent.com/120498080/220193081-4fd32cf8-2f18-4349-8c4a-9069564219f8.png)
 
    
### CRPR-Clock Reconvergence Pessimism Removal
 
![Screenshot (2479)](https://user-images.githubusercontent.com/120498080/220192392-95da6514-59d8-4362-b9d3-806b0c0a5d5a.png)
- During decision of max and min launch path sometimes there is confussion in choosing the best path because some part of the circuit is common in two paths and that can be soled by CRPF    
- Clock reconvergence pessimism (CRP) is a delay difference between the launching and capturing clock pathways. The most prevalent causes of CRP are convergent pathways in the clock network and varying minimum and maximum delays of clock network cells.  
 

### STA Text Report
When STA tool dose analysis and reports setup and hold timing, its first converts that logic into nodes and cells and archs.
  
![Screenshot (2438)](https://user-images.githubusercontent.com/120498080/220064935-353bce95-62b9-43e4-81ae-871a6a9e0994.png)

#### Sample STA Text Report
![Screenshot (2441)](https://user-images.githubusercontent.com/120498080/220065230-b7350659-e6a7-4edc-92ac-ae32057a372f.png)    
    
    
    
## Day 4
---
### Crosstalk and Noise
Crosstalk is any phenomenon in electronics that occurs when a signal carried on one circuit or channel of a transmission system causes an undesirable effect in another circuit or channel.
    
![Screenshot (2487)](https://user-images.githubusercontent.com/120498080/220415424-0f54faea-a132-4c85-a22b-b8d0464570fe.png)
- In this when both aggressor is changing from 0 to 1 and victim is also changing in the same direction so it can couse the victim signal to change faster, hence rise time changes and **delay decreses**
- If the aggressor and victim are changing in opposite direction then because of coupling caps aggressor will try to push the victim signal in same direction, hence chage becomes slower in victim which in **increases delay**
- STA take this changes into consideration and accordingly calculate setup and hold time.
    
![Screenshot (2488)](https://user-images.githubusercontent.com/120498080/220418621-7cb980de-6442-43f0-b403-faf870126c88.png)
- When aggressor is changing whereas victim signal in not changing so we can have glich in the victim signal which could lead to functional failure. So STA need to make sure these glitches are not present or should be below a threshold value.
    
### Process Variation
- Sometimes we have process variation(can have differnt delay or transition time)within a chip or from wafer to wafer or within the wafer. So STA need to take this into account.
    
![Screenshot (2489)](https://user-images.githubusercontent.com/120498080/220419851-f8659c13-246d-432a-8564-11ef1a2baaa9.png)
    
### Clock Gating Checks 
- A clock gating check occurs when a gating signal can control the path of a clock signal at a logic cell. For example when a clock and enable signal is fedded as input to an AND gate.
- The signal must be used as clock downstream, like feed a flop or latch clock pin or feed output port or feed generated clock.
- Intention of this check is that transition on gating pin does not Create unnecessary active edge of the clock in the fanout.

![Screenshot (2490)](https://user-images.githubusercontent.com/120498080/220424817-99037fb4-c0bb-405d-b2e4-74bdf5905a5d.png)
- So in case of AND and NAND gates clock gating checks that during setup check the enable should become stable sometime before the clock rising edge(so that coack has enough setup time) and during hold check the enable should become stable sometime after the clock rising edge
    
![Screenshot (2491)](https://user-images.githubusercontent.com/120498080/220424864-58615adc-4b19-4732-9ce3-e7a1642388a4.png)
- So in case of OR or NOR gates clock gating checks that before some time and after some time of the low value of the clock enable pin should be stable.
    
Sometimes tools cannot set this clock gating checks and gives error, so we need to do it manually by suing this commands
```
   set_clock_gating_check
```
  
### Checks on Async Pins
- Async Pins in the designes are reset or clear pins 
- These checks are needed only of asynchronous pins not for synchronous pins(beacure synchronous reset pins come from flops which is alraedy synchronised )
    
![Screenshot (2494)](https://user-images.githubusercontent.com/120498080/220427042-5de0431f-4639-4553-8c9f-7ea7a52b59aa.png)
- Assertion means clear is on and outputs are zero, which is indepeant of clock
- De-assertion means clear is off and output is dependent on input and clock.
    
![Screenshot (2495)](https://user-images.githubusercontent.com/120498080/220429740-d796001e-ca66-4ab3-8176-9d6c1b826108.png)
![Screenshot (2496)](https://user-images.githubusercontent.com/120498080/220429770-7cfc8dbe-dcb9-4ea4-893c-9dca6d6d289e.png)
    
- Assertion to De-assertion change should not happen very close to clock edge, it should happen sometime before clock edge and sometime after clock edge which is known as the **Recovery Time** and this time requirement is given by the logival liblary of the flop 
    
## Day 5
---
### Clock Groups    

    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
