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

Day 2 workshop focused on chip floor planning and placement, where we learnt about unitization facor, aspect ratio, pre-placed cells, de-coupling capacitors, need for libraries and characterization, cell design flow with the help of inverter and general timing perameters like threshold and propagation delay. Also gained an hands on experience by implementing floorplan and palcement for picorv32a design using OpenLane.

**Take aways of Day 2**

**1. Determaining aspect ratio :**

       Aspect Ratio = Height of the core /Weidth of the core
   * If aspect ratio = 1 (Chip is square) else (Chip is rectangle)
  
**2. Utilization Factor :**
  
       Utilization Factor = Area occupied by the netlist/Total area of the core
     
     
   * If all logic cells occupies the complete area of the core => Utilization factor = 1
  
**3. Preplaced cells :**
 
   * These cells are placed only once and reused multiple times

   * The automated placement and routing tools doesn't touch the locations of preplaced cells.

**4. Decoupling Capacitor :**

   * Gives power supply to preplaced cells locally, inorder to prevent undefined logic state.

**5. Power planing :**

   * Due to power supply at single place Ground Bounce and Voltate doop may be occures near placed cells and in buses to avoid that we can use power supply with multiple vdd and      vss lines, for example Mesh power planning.
  
 **6. Pin placement :**
 
   * Bigger the size smaller the resitance; clock paths should be least resistive. Therefore, clock pins are bigger in size
 
 **7. Flooplan :**
 
   * Standard cell placement happens in placement stage not in floorplan. Here chip dimensions are determained using size of logic less in the netlist.


 **8. Placement :**
 
   * Buffers are placed between inputs and cells to reduce the slew and save signal integrety.

   * We do abatement for some logic to reduce the time delay and to increase the performance for perticular section.   
 
 **9. Library characterization and modeling :**
 
   * Library has  standard cells with differnt functionalality, different sizes, different vth. 
   
   ![Screenshot (7)](https://user-images.githubusercontent.com/100410542/155857917-04942deb-1885-4174-9ad6-f315ef62e8a3.png)
   
   **LABS**
   
  **1. Floorplan using OpenLane**
  
     Floorplan variables exploration in README.md
     
  ![image](https://user-images.githubusercontent.com/100410542/155859249-c9266ff3-1d6d-4777-834b-7bb8494ff091.png)
  
     Flooeplan. tcl file oberving default values for the switches
     
  ![image](https://user-images.githubusercontent.com/100410542/155859336-0d183516-a656-43a5-92fc-03f00600e0b9.png)
  
     run_floorplan
     
   ![image](https://user-images.githubusercontent.com/100410542/155859452-68646dfe-3201-4f49-bdd9-db4acc87ba9a.png)
 
    runs folder
    
  ![image](https://user-images.githubusercontent.com/100410542/155859588-025daa52-9f45-406c-876e-884938389041.png)
  
  **Magic tool to explore Layout of floorplan**

      magic -T /home/prudhvivnkt/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &

![image](https://user-images.githubusercontent.com/100410542/155859724-88bcca80-6000-45f0-b350-901bc1783b5a.png)
    
 ![image](https://user-images.githubusercontent.com/100410542/155859763-dac48d16-3eb5-4252-9f08-21907c7bc4a9.png)
 
 ![image](https://user-images.githubusercontent.com/100410542/155859912-f1a79709-af81-476d-8cc0-fb79cfa51285.png)
 
 
 ![image](https://user-images.githubusercontent.com/100410542/155859930-b3ade0a3-b68b-4a72-97bc-f73d301354d4.png)
 
     congestion placement    run_placement
     
 ![image](https://user-images.githubusercontent.com/100410542/155859991-5b2b42ec-e1ff-4269-9759-aca3c953436a.png)

**Magic tool to explore Layout of placement**

      magic -T /home/prudhvivnkt/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def
  
  ![image](https://user-images.githubusercontent.com/100410542/155860062-4191171b-e214-4f09-b3bb-212cf3241568.png)


  **We also learnt about cell design flow with listing out Inputs, Design steps and Outputs with the help of the Inverter after having the knowledge of designing library the day has ended giving breif details of characteization flow stepts involved and use of GUNA software for generating timing, noise and power models**
  
  
  ![Screenshot (9)](https://user-images.githubusercontent.com/100410542/155860108-24b54b97-6003-40d7-8b07-a9c705d050f7.png)




  
  
     
 
 
    
    


 
 
 

 
