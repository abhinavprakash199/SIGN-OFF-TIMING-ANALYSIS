# SIGN-OFF-TIMING-ANALYSIS

This repository contains the whole summary of hands on done by Abhinav Prakash (IS22MTECH14002) during the workshop SIGN-OFF-TIMING-ANALYSIS-using-OpenLane-SKY130 organised by VSD. The full STA flow was implemented using OpenSTA with SKY130nm PDK.

<p align="center">
    <img src="https://user-images.githubusercontent.com/120498080/219842863-80a8bcc6-60cc-4f75-a2ec-f874641fd9bf.png">


* [Day 1](#day-1)
    + [Lab 1](#Lab-1)
        - [OpenSTA](#OpenSTA)
        - [Constraints Creation](#Constraints-Creation)
        - [Run OpenSTA](#Run-OpenSTA)
* [Day 2](#day-2)
    + [Lab 2](#Lab-2)
        - [Liberty Files](#Liberty-Files)
        - [Exercise 2.1](#Exercise-2.1)
        - [SPEF Files](#SPEF-Files)
        - [Exercise 2.2](#Exercise-2.2)
* [Day 3](#day-3)
    + [Lab 3](#Lab-3)
        - [Exercise 3.1](#Exercise-3.1)
* [Day 4](#day-4)  
    + [Lab 4](#Lab-4)
* [Day 5](#day-5)   
    
* [Appendix](#Appendix)
* [References](#references)
* [Acknowledgement](#acknowledgement)
* [Inquiries](#inquiries)


## Day 1
--- 
- [Day 1 Theory](https://github.com/abhinavprakash199/SIGN-OFF-TIMING-ANALYSIS/blob/main/THEORY.md#day-1) 

### Lab 1 
---
First download the working folder
```
git clone https://github.com/vikkisachdeva/openSTA_sta_workshop    
```    
### OpenSTA
OpenSTA is a gate level static timing verifier. As a stand-alone executable it can be used to verify the timing of a design using standard file formats.
- Verilog netlist
- Liberty library
- SDC timing constraints
- SDF delay annotation
- SPEF parasitics
    
OpenSTA is a Static Timing analysis (STA) tool that takes design, standard cell, constraints as input and perform timing checks on the design.

### Terminal
![image](https://user-images.githubusercontent.com/120498080/220437164-b14afedf-d730-4f1a-99e3-d6da9356ddc3.png)

### Inputs to OpenSTA
1. Netlist format(Usually provided in Verilog using read_verilog command)
    
#### Standard cells or lib cells instantiations in `simple.v` file
![1](https://user-images.githubusercontent.com/120498080/220438110-b1d41580-b78a-4de5-aa63-850bac88bbb9.PNG)

  
2. Standard Cells (Provided in .lib format{`sky130_fd_sc_hd_tt_025C_1v80.lib`} using `read_liberty` command). A typical cell will have 
- Input ports definitions
- Output port definitions
- Functionality of the cells
- Input to Output relationship
 
### Liberty Files
![image](https://user-images.githubusercontent.com/120498080/220438774-7871a7d4-54b2-4318-824b-c46847a242e1.png)
### Input and Oputput of Liberty Files
![2](https://user-images.githubusercontent.com/120498080/220439330-5e8ad720-d945-4a0d-aa1e-f421dd609d63.PNG)

3. Timing constraints(Provided in sdc format using `read_sdc` command)

### Constraints Creation
1. Clocks are defined using `create_clock` command (Lets define a clock with period 50 on port tau2015_clk)
```
    create_clock –period 50 –name tau2015_clk [get_ports tau2015_clk]
```
    
2. IO delays(Primary ports are defined with delays with associated clock: tau2015_clk)
```
    set_input_delay 5 –max –rise [get_ports inp1] –clock tau2015_clk
    set_output_delay -10 –min –fall [get_ports out] –clock tau2015_clk
```
3. Input transitions by environmental factors
```    
    set_input_transition 10 –min –rise [get_ports inp1]
```
4. Capacitive load on output pin
 ```
    set_load –pin_load 4 [get_ports out]
```
#### The `simple.sdc` file 
- Which consists of the clock period, IO delays, Input transition and Capacitance Delays.
![3](https://user-images.githubusercontent.com/120498080/220440580-7594b0fd-8d62-4eb0-b848-852656bc266b.PNG)

### runScript
- In runscript you can define all the commands you want to run in the openSTA tool
- Tool will execute each command sequentially in order(There are some commands which are executed in parallel in some cases)
- Runscript is in `.tcl` format.
#### `run.tcl` script
![4](https://user-images.githubusercontent.com/120498080/220449538-4531e191-1f9d-4488-b980-f3c45c4ca259.PNG)
- `report_checks` command is used to report timing on the design.   
### Run OpenSTA
• Run openSTA using command 
```
    sta run.tcl -exit | tee run_1.log
```
#### Terminal
![image](https://user-images.githubusercontent.com/120498080/220450105-3c40f769-d74c-459b-bc6c-3879851e2ada.png)
    
#### Output of run OpenSTA `run_1.log` file
![5](https://user-images.githubusercontent.com/120498080/220454794-4a8422c0-27dd-47f4-b7c9-3503e1d0dda8.PNG)
![6](https://user-images.githubusercontent.com/120498080/220460662-f89a6f4b-3ceb-409f-8106-42e3a564f07c.PNG)


- [Reference for Lab 1](https://drive.google.com/file/d/1L-zJrvT8lbbpEXGdJ0c0WO1ymvrw-lnc/view?usp=sharing)
 
## Day 2
---  
- [Day 2 Theory](https://github.com/abhinavprakash199/SIGN-OFF-TIMING-ANALYSIS/blob/main/THEORY.md#day-2)

### Lab 2
--- 
### Liberty Files
The .lib file is an ASCII representation of the timing and power parameters associated with any cell in a particular semiconductor technology.

The .lib file contains timing models and data to calcumax
- I/O delay paths
- Timing check values
- Interconnect delays

![Screenshot (2442)](https://user-images.githubusercontent.com/120498080/220071960-0befd39b-4bb0-4bd5-9a0a-534e61e9d6a4.png)
![Screenshot (2443)](https://user-images.githubusercontent.com/120498080/220071978-5fbfbdc2-959c-499c-8622-eb17b4904b9f.png)
![Screenshot (2444)](https://user-images.githubusercontent.com/120498080/220072001-bb8e1afa-aa6d-4724-b86e-7581d4519391.png)
### Exercise 2.1
---
#### Q1) Find all the cells in simple_max.lib
    
To find no of cells in `simple_max file` use this command 
```
    more simple_max.lib | gep -c " End cell"
```
- Tip to list all cells in a liberty in a `all_cell` file     
```
    more simple_max.lib | grep " End cell" | tee all_cell
``` 
#### Terminal
![image](https://user-images.githubusercontent.com/120498080/220457376-1ee44af2-0656-4ce1-9717-97811d320754.png)
    
#### Q2) Find all the pins of the cell NAND2_X1 in `simple_max.lib`
- It has 1 output and 2 input pins.
![7](https://user-images.githubusercontent.com/120498080/220459325-b834ef64-db7c-42f2-90aa-cb4ba7700809.PNG)
![8](https://user-images.githubusercontent.com/120498080/220459355-661982c3-cf51-4314-9c96-b703e01224a3.PNG)


#### Q3) What difference you see between NAND2_X1 and NAND3_X1
- NAND2_X1 is 2 input nand gate and NAND3_X1 is 3 input nand gate.
- Also Capacitance values are differnet.
    
![10](https://user-images.githubusercontent.com/120498080/220462179-acba40b2-5562-47b4-947a-5f9ed86f592b.PNG)

#### Q4) What is the difference between ‘simple_max.lib’ and ‘simple_min.lib’
- I compared the file and found they have differnet values in `cell_ fall`, `fall_transition`, `cell_rise` and `rise_transition`of all the cells.
- Variations in the fabrication process could could either increase or decrease the delay pf cells. Timing analysis must take into account the min and max values of delay in liberty files. These libraries will be used by the STA tool for analysis.
- These respresent the table which are calculated based on several equations and tells about paramenter like have different slope of rise and fall time delay, etc.
- It specify PVT variation in max and min conditions.
![Screenshot (2448)](https://user-images.githubusercontent.com/120498080/220101858-5751278d-65e7-46b7-a543-5dd9ef201d4d.png)
  
### SPEF Files
- A SPEF (Standard Parasitic Exchange Format) file describes parasitic information of the design like delay, setup and hold time of the flops based on our design(these are not general values). 
- Users would never create this file manually.
- It is automatically generated by the place and rout tool(I can also be write and read STA tool).
- It is mainly used to pass parasitic information from one tool to another.
- In OpenSTA command to write spef files is 
```
    write_spef
```
- In OpenSTA command to read spef files is 
```
    raed_spef
```    
A Typical SPEF File has 4 main sections
1. Header
2. Name Map
3. Top Level Ports
4. Parasitic description

#### `simple.spef` file
![11](https://user-images.githubusercontent.com/120498080/220463200-40ed044e-f79a-4f41-956a-f0a7a6d6cf7d.PNG)  
#### Standard cells or lib cells instantiations in `simple.v` file
![12](https://user-images.githubusercontent.com/120498080/220463786-c516a974-fb57-4786-8625-7b2f4040e406.PNG)
#### `run.tcl` script
![13](https://user-images.githubusercontent.com/120498080/220525061-32e3237f-2807-45d3-b3e1-3115226bb5c1.PNG)

### Run OpenSTA
• Run openSTA again using command 
```
    sta run.tcl -exit | tee run.log
```
### Exercise 2.2
---
#### Understand other paths in `run.log`
- `report_checks` command is used to report timing on the design.
#### `run.log` script
![image](https://user-images.githubusercontent.com/120498080/220098148-a37771fe-6d8e-4b26-a5c0-571fb9570287.png)

### Increase number of paths reported and analyze those    
    
- [Reference from Lab 2](https://drive.google.com/file/d/1A2SBqSQWAvsr65dGsNvp4cHjgwkc2uql/view?usp=sharing)
 
## Day 3
---
[Day 3 Theory](https://github.com/abhinavprakash199/SIGN-OFF-TIMING-ANALYSIS/blob/main/THEORY.md#day-3)
    
### Lab 3
---
#### Circuit
![Screenshot (2484)](https://user-images.githubusercontent.com/120498080/220355115-dacde325-1a7d-4f77-9962-16c9f2c98531.png)

#### `v27.v` file
![image](https://user-images.githubusercontent.com/120498080/220355350-65fdf476-d4e4-4071-a605-342b91ac0e7e.png)
  
#### `run.tcl` script
![image](https://user-images.githubusercontent.com/120498080/220355084-298ccc5e-019b-457c-bb93-d22d62a65df6.png)

- Run STA with command
```
    sta run.tcl -exit | tee run.log
```
#### Generated `run.log` file
![image](https://user-images.githubusercontent.com/120498080/220355776-c4bc0104-0455-4cc0-a5df-c2c6f8692c16.png)

### Exercise 3.1
---
#### Change the number of paths being reported to 100 and analyze each path in detail and understand 
```
    report_checks –from F1/CK -endpoint_count 100
```
#### Changed `run.tcl` script     
![image](https://user-images.githubusercontent.com/120498080/220357030-340ed845-fddb-4ccd-833c-5eb337fc3e99.png)
 
- Run STA with command    
```    
    sta run.tcl -exit | tee out.txt    
```  
#### Generated `out.txt` file
![image](https://user-images.githubusercontent.com/120498080/220357408-0edbd63c-b018-48f7-8451-af7a8cbb162b.png)
- This will created the timing report of all the possible path combination and edge triggered with differnet combination.
- Here are the few comparision reports of duffernet timing paths.
![Screenshot (2481)](https://user-images.githubusercontent.com/120498080/220358189-9332cf1d-f50a-4d73-85ab-617b74472f9b.png)
![Screenshot (2483)](https://user-images.githubusercontent.com/120498080/220358230-597b252b-34a8-48f2-8508-a7f5a5b8393c.png)
    
- [Reference from Lab 3](https://drive.google.com/file/d/15GElaM4zCY1tjLTkZI6W2D4XMQhhLtl4/view?usp=sharing)
    
## Day 4
---
[Day 4 Theory](https://github.com/abhinavprakash199/SIGN-OFF-TIMING-ANALYSIS/blob/main/THEORY.md#day-4)
    
## Lab 4    
---
- [Reference from Lab 4](https://drive.google.com/file/d/18A2-AUr6HjYo7iSLXuEEQjyHcmYkpfdh/view?usp=sharing)
    
    
    
```   
  report_checks -path_delay min_max -fields {nets cap slew input_pins} -digits {4} 
```
    
    
    
    
    
    
    
    
    
    
    
    
    
## Day 5
---
[Day 5 Theory](https://github.com/abhinavprakash199/SIGN-OFF-TIMING-ANALYSIS/blob/main/THEORY.md#day-5)   
    
    
## Lab 5
---
- [Reference from Lab 5](https://drive.google.com/file/d/1oaHP7CLQrpXiUlowJXuGKhF0kTbOAmht/view?usp=sharing)

    
    

## Appendix
---
    
## References
---
- https://www.synopsys.com/glossary/what-is-static-timing-analysis.html
- https://github.com/Anmol-wq/VSD-IAT-Sign-off-Timing-Analysis---Basics-to-Advanced
- [OpenSTA installation guide](https://github.com/The-OpenROAD-Project/OpenSTA)

    
## Acknowledgement
---
Finally, I would like to express my sincere gratitude to [Kunal Ghosh](https://www.linkedin.com/in/kunal-ghosh-vlsisystemdesign-com-28084836/){Co-founder of VLSI System Design (VSD) Corp. Pvt. Ltd.} and [Nickson Jose, VLSI Engineer](https://www.linkedin.com/in/nickson-jose/) {ASoC Physical Design Engineer} for tremendous assistance in planning and presenting this workshop on Advanced-Physical-Design-using-OpenLane-SKY130. The workshop was excellent and well designed, this workshop taught me a lot of new things about the design of GDSII file from RTL using OpenLANE, open source tool.   

## Inquiries
- Connect with me at [LinkedIn](https://www.linkedin.com/public-profile/settings?trk=d_flagship3_profile_self_view_public_profile)

