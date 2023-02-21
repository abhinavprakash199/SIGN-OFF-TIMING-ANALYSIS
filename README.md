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
 
### Library Files
![image](https://user-images.githubusercontent.com/120498080/220438774-7871a7d4-54b2-4318-824b-c46847a242e1.png)
### Input and Oputput of Library Files
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
![image](https://user-images.githubusercontent.com/120498080/219945663-32af476f-6ab2-4e6f-b099-100624519cf0.png)
- `report_checks` command is used to report timing on the design.   
### Run OpenSTA
• Run openSTA using command 
```
    sta run.tcl -exit | tee run.log
```
    
#### Output of run OpenSTA
![image](https://user-images.githubusercontent.com/120498080/219946164-6859b94d-1138-4dc8-9e54-6b5515855da0.png)
![image](https://user-images.githubusercontent.com/120498080/219946136-f627ba62-19f7-4d1c-b098-56648db7c89b.png)

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
#### Find all the cells in simple_max.lib
- NAND2_X1 cells
 ![image](https://user-images.githubusercontent.com/120498080/220090123-4b162606-b529-4a1d-b5c8-c289f54b3a83.png)

#### Find all the pins of the cell NAND2_X1 in simple_max.lib
- It has one output and 3 imput pins
![image](https://user-images.githubusercontent.com/120498080/220090421-bfa87d2d-25fe-4160-9195-41d7a8ccc69e.png)

#### What difference you see between NAND2_X1 and NAND3_X1
- NAND2_X1 is 2 input nand gate and NAND3_X1 is 3 imput nand gate.
#### What is the difference between ‘simple_max.lib’ and ‘simple_min.lib’
- I compared the file and found they have differnet values in `cell_ fall`, `fall_transition`, `cell_rise` and `rise_transition`of all the cells.
![Screenshot (2448)](https://user-images.githubusercontent.com/120498080/220101858-5751278d-65e7-46b7-a543-5dd9ef201d4d.png)
  
### SPEF Files
- A SPEF (Standard Parasitic Exchange Format) file describes parasitic information of the design like delay, setup and hold time of the flops based on our design(these are not general values). 
- Users would never create this file manually.
- It is automatically generated by the place and rout tool(I can also be write and read STA tool).
- It is mainly used to pass parasitic information from one tool to another.   
- In OpenSTA command to write spef files is `write_spef`
    
A Typical SPEF File has 4 main sections
1. Header
2. Name Map
3. Top Level Ports
4. Parasitic description

#### `simple.spef` file
![image](https://user-images.githubusercontent.com/120498080/220094978-626dff99-a9c2-4509-ae52-4c81443b9621.png)    
#### Standard cells or lib cells instantiations in `simple.v` file
![image](https://user-images.githubusercontent.com/120498080/220113317-975ec099-319d-4eab-af30-d05c1f2f2e1b.png)
#### `run.tcl` script
![image](https://user-images.githubusercontent.com/120498080/220094332-4669744a-974e-44ef-9dc9-b4a64b5569e9.png)

### Run OpenSTA
• Run openSTA again using command 
```
    sta run.tcl -exit | tee run.log
```
### Exercise 2.2
---
#### Understand other paths in `run.log`
- `report_checks` command is used to report timing on the design.
#### `run.tcl` script
![image](https://user-images.githubusercontent.com/120498080/220098148-a37771fe-6d8e-4b26-a5c0-571fb9570287.png)

    
#### Understanding Timing Reports
![Screenshot (2445)](https://user-images.githubusercontent.com/120498080/220099771-cb481c96-2e5d-4fd3-86c0-05d6facee4e9.png)

    
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

