## ADVANCED PHYSICAL DESIGN USING OPENLANE/SKY130

![image](https://user-images.githubusercontent.com/100410542/155698323-8ce96528-ac64-41cc-9d66-bf16a9c64e09.png)

**Overview**

In Intergrated Circuit (IC) design, physical design is the most challenging and core past of the cycle. Physical design follows next to circuit design. Physical Design is known as back-end design with RTL netlist, library information on the basic devices in the design and technology file containing the manufacturing constraints as the inputs and concluding with Layout Post Processing. Here a complete Physical Design is implemented using openlane flow, a fully-automated RTL2GDSII flow with the help of Google-SkyWater's open source 130nm PDK (process design kit). 

![image](https://user-images.githubusercontent.com/100410542/155697182-e014e5cd-a172-41e0-a5d6-a3ecd6452fb4.png) 

# CONTENTS
**1. Inception of open-source EDA, OpenLANE and Sky130 PDK**
 * How to talk to computers
 * SoC design and OpenLANE
 * Starting RISC-V SoC Reference design
 * Get familiar to open-source EDA tool
 
**2. Understand importance of good floorplan vs bad floorplan and introduction to library cells**
 * Chip Floor planning considerations
 * Library Binding and Placement
 * Cell design and characterization flows
 * General timing characterization parameters
 
**3. Design and characterize one library cell using Magic Layout tool and ngspice**
 * Labs for CMOS inverter ngspice simulations
 * Inception of Layout – CMOS fabrication process
 * Sky130 Tech File Labs
 
**4. Pre-layout timing analysis and importance of good clock tree**
 * Timing modelling using delay tables
 * Timing analysis with ideal clocks using openSTA
 * Clock tree synthesis TritonCTS and signal integrity
 * Timing analysis with real clocks using openSTA
 
**5. Final steps for RTL2GDS**
 * Routing and design rule check (DRC)
 * PNR interactive flow tutorial
 ## Day 1 : Understanding of OpenLane Flow, open source PDK files and running synthesis
 In day 1 we learnt the importance of Physical design and OpenLane flow to automate RTL2GDSII flow.
 The day started with the importance of Hardware discription languages, instruction sets and layouts in communication with computers and ended with the synthesis of picorv23 design  using yosys and abc
 
 **How to communicate with computers**
 The application software written using high level languages such as c,c++,java are compiled to instriction set (RISC-V core computer uses RISC-V ISA) assembly language. These instructions are converted to binary language and excecuted in layout. Hardware discription languages are used to implement the instructions which serves as an interface between layout and instruction set. In addition to Physical Design flow we also understand the importance and difference of Foundary IP's ans Macros
 
 ![image](https://user-images.githubusercontent.com/100410542/155702267-3c371040-2403-40c2-8b19-e30611d06c14.png)
 
 * For OpenLane flow design/src/*.v and PDK files are given as inputs to generate GDSII
 
 ![image](https://user-images.githubusercontent.com/100410542/155707174-9ec9de51-3aa9-48d4-b36c-6ece89204fb7.png)
 
 * OpenLane Design Stages

         RTL Synthesis, Technology Mapping, and Formal Verification : yosys + abc

         Static Timing Analysis: OpenSTA
         
         Floor Planning: init_fp, ioPlacer, pdn and tapcell

         Placement: RePLace (Global), Resizer and OpenPhySyn (formerly), and OpenDP (Detailed)

         Clock Tree Synthesis: TritonCTS

         Fill Insertion: OpenDP/filler_placement

         Routing: FastRoute or CU-GR (Global) and TritonRoute (Detailed)

         SPEF Extraction: SPEF-Extractor (formerly), OpenRCX

         GDSII Streaming out: Magic and Klayout

         DRC Checks: Magic and Klayout

         LVS check: Netgen

         Antenna Checks: Magic

         Circuit Validity Checker: CVC
         
 * Command Flow
 
       prep design

       run_synthesis

       run_floorplan

       run_placement

       run_cts

       gen_pdn

       run_routing

       run_magic

       run_magic_spice_export

       run_magic_drc

       run_lvs

       run_magic_antenna_chec
         
* Exploring OpenLane directory

![image](https://user-images.githubusercontent.com/100410542/155711974-4568b387-71e9-4f4d-90bc-f61e6ad35a00.png)

 * Configuration directory
 
 This directory has .tcl files that are used for default perameters
 
 ![image](https://user-images.githubusercontent.com/100410542/155712108-9d8eb9a3-334c-40dd-abd2-6c669cbab2bf.png)
 
 * picrorv32a directory 
 
 This is the design we are implementing OpenLane flow
 
 This directory has src file, SCL, and config.tcl where we can overwrite the default configurations
 
 runs directory is added after initiating the flow.
 
 ![image](https://user-images.githubusercontent.com/100410542/155712629-ebc1514f-7481-4aa6-96c2-864e024bfab0.png)
 
 * runs directory of the picorv32a design
 
 ![image](https://user-images.githubusercontent.com/100410542/155713289-54f8629d-52fb-4d6f-a321-4c624818e3ae.png)


         
* Invoke openlane

    docker

* Step by step excecution of OpenLane flow

    ./flow.tcl -interactive
    
![image](https://user-images.githubusercontent.com/100410542/155709717-43a3c216-0646-4bbe-968e-7cee3a2521d3.png)

* Importing all the packages

    package require openlane 0.9

* setup design

    prep -design picorv32a
    
![image](https://user-images.githubusercontent.com/100410542/155710683-9af087dc-be71-4d6c-b275-2794579bb4f6.png)

* rununing synthesis

This is calles both yosys and abc

    run_synthesis

![image](https://user-images.githubusercontent.com/100410542/155714996-da34396f-a7d7-4717-a45f-4762b96ae22b.png)


* Flop ratio of the design picor32a


![image](https://user-images.githubusercontent.com/100410542/155715225-93987b92-9ad5-4292-a356-bba640677b17.png)

![image](https://user-images.githubusercontent.com/100410542/155715256-0ac6c5b4-ec3d-4829-aace-2bfea5c268d6.png)

      Flop ratio = flop count/no.cells = 0.108429

##  Day 2 : Floor planing


    
    
 
 
    
    


 
 
 

 
