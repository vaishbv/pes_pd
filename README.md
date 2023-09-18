# pes_pd
# ADVANCED PHYSICAL DESIGN USING OPENLANE/SKY130

# Contents

## Day 1
- [How to Talk to Computers](#how-to-talk-to-computers)
- [Introduction to OpenLANE and Strive chipsets](#introduction-to-openlane-and-strive-chipsets)
- [Getiing Familiar with the Open Source EDA Tools](#getting-familiar-with-the-open-source-eda-tools)

## Day 2
- [Chip Floor Planning Considerations](#chip-floor-planning-considerations)
- [Library Binding and Placement](#library-binding-and-placement)
- [Cell Design and Characterization Flows](#cell-design-and-characterization-flows)
- [General timing characterization parameters](#general-timing-characterization-parameters)

## Day 3
- [Labs for inverter ngspice simulations](#labs-for-inverter-ngspice-simulations)
- [Inception of Layout and CMOS Fabrication Process](#inception-of-layout-and-cmos-fabrication-process)
- [Sky130 Tech File Labs](#sky130-tech-file-labs)

## Day 4
- [Timing Modelling using Delay Tables](#timing-modelling-using-delay-tables)
- [Timing Analysis with Ideal Clocks using OpenSTA](#timing-analysis-with-ideal-clocks-using-opensta)
- [Clock Tree Synthesis TritonCTS and Signal Integrity](#clock-tree-synthesis-tritoncts-and-signal-integrity)

## Day 5
- [Power Distribution Network and Routing](#power-distribution-network-and-routing)
- [SPEF Extraction](#spef-extraction)




## DAY 1- Inception of open-source EDA, OpenLANE and Sky130 PDK

## How to talk to Computers
**Introduction to QFN-48 Package, chip, pads, core, die and IPs**

Lets us discuss this with an Aurdino board as an example.
An Arduino board is a popular open-source electronics platform that consists of a microcontroller, development environment. It is a small computer chip that processes instructions and controls the behavior of your electronic project.Arduino boards work by providing a platform for you to write and upload code that controls the behavior of the microcontroller on the board.

![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/3c9c1cb2-abef-4592-9456-4f93f9224a1d)

This board can describe it in a form of a block diagram:

![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/9a3fb263-e0bd-4a43-96c4-6cf5bfe105e8)

- This is the typical board diagram with the processor and all the interfaces

the IC it looks something like this
![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/4eb85a39-26e1-4977-b477-e1954dbf552a)

- This IC contains the QFN-48(Quad Flat No-Leads) 7nmx7nm package
-  contains 48 electrical connections or pins
-  pin locations of the ackage are given by the arduino board.

The main chip is located at the centre of the package and is connected to the pins by wire bounds.
These wire bounds trasnfer all the incoming signals to the chip

![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/edf8e92c-6063-416f-b96f-9ffdb2548fc8)

The chip in the package when opened up looks like this:

![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/85a1f1d3-c0e4-4b5c-ba93-76eee42fe166)

-  PADs in chip are like the  metal  points on the bottom of the chipthat are  used to connect the chip to a circuit through signal can be sent into the chip.
-  Core of a chip is  the central part that  processes the information.Its a place where all our Digital logical sitslike the AND gate,OR gate,MUXs,etc.
-  Die s a tiny, flat piece of silicon that contains the actual electronic circuits and defines the size of the chip. The  Die is manufactured on a Silicon wafer.

The core of the CHIP contains  an SoC SRAM,ADCs,DACs,PLL,SPI and some momre components.

![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/439258ad-5afa-497d-8b8c-50b34274dcb1)

- the SRAM,ADC,DAC,PLL  are known As Foundry IP's.
-  a foundry refers to a specialized facility or company that manufactures integrated circuits (ICs) or microchips.
-  the SoC and the SPI are basically called as Macros.

  
  **Introduction to RISC-V**

  RISC-V is an open-source instruction set architecture (ISA) for designing and implementing custom microprocessors and other hardware.
- HIGH LEVEL CODING LANGUAGE Like C or C++
- Complied to RISCV assembly language program converted to binary level logic 1 and logic 0
- HARDWARE DESCRIPTION LANGAUGE implements the RISCV assembply program using an RTL(register transfer logic)
- RTL converted to layout (GDS - Graphic Data System)

![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/ca4052e8-bdc3-474f-ae66-4e7e53449c1d)

**From Software Applications to Hardware**
- Apps: Application software is a type of computer software that is designed to perform specific tasks or functions for end-users.

- System software: System software refers to a category of computer software that acts as an intermediary between the hardware components of a computer system and the user-facing application software. It provides essential services, manages hardware resources, and enables the execution of application programs.

- Operating System: The operating system is a fundamental piece of software that manages hardware resources and provides various services for both users and application programs. It controls tasks such as memory management, process scheduling, file system management, and user interface interaction.

- Compiler: A compiler is a type of software tool that translates high-level programming code written by developers into assembly-level language.

- Assembler: An assembler is a software tool that translates assembly language code into machine code or binary code that can be directly executed by a computer's processor.

- RTL: RTL serves as an abstraction level in the design process that represents the behavior of a digital circuit in terms of registers and the operations that transfer data between them.

- Hardware: Hardware refers to the physical components of a computer system or any electronic device. It encompasses all the tangible parts that make up a computing or electronic device and enable it to perform various tasks.

![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/1609a405-fae2-4670-9361-bea50432ae4a)



**SoC design and OpenLANE**

![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/9efd82d1-2a6d-42a9-b830-bdddff55bc2a)


PDK(Process Development Kit)
A Process Development Kit (PDK) is a set of tools, libraries, and data files provided by semiconductor manufacturers to integrated circuit (IC) designers. PDKs are essential for IC designers because they contain information and resources specific to the manufacturer's semiconductor fabrication process.
PDK contains
- Technology Files: PDKs include technology files that describe the manufacturing process details, such as transistor models, device parameters, interconnect layers, and design rules.
- Design Libraries: PDKs contain design libraries, which are collections of pre-designed and characterized circuit components, such as standard cells, memory elements, and analog circuit blocks.
- Simulation Models: PDKs provide simulation models for various components and elements used in IC designs.
- Layout and Mask Information: PDKs offer layout and mask design information, which is essential for creating the physical layout of the IC.
- Design Rule Checking (DRC) and Extraction Tools: PDKs often include tools for checking designs against manufacturing-specific design rules (DRC) and for extracting parasitic parameters that affect circuit performance.
- Calibration Data: Some PDKs include calibration data to ensure that the design tools accurately reflect the manufacturing process's characteristics.
- Documentation: Comprehensive documentation is typically part of a PDK, providing designers with information on how to use the kit effectively and design guidelines.


**Simplified RTL2GDS flow**

![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/4fe32ecc-076b-4414-abe0-6f150129907a)

- Synthesis: is the process of converting the RTL description of a digital design into a gate-level netlist. This netlist consists of logical elements (gates) and their interconnections.

 ![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/bdf8b089-cd8e-4320-88d5-305e71f0a9f7)


- Floor/Power Planning: In this phase, the chip's overall floorplan is defined. It determines the approximate locations of key components, such as blocks and macros, and how power is distributed across the chip.
  
  - Macro Floor Planning - We define the macro dimensions, pin locations and rows are defined.
 
  ![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/b0e43593-ec5c-47a6-b7f0-51e70c9fb8e1)

  - Chip Floor Planning - Partition the chip die between different system building blocks and place the I/O pads.


  - Power Planning - The power distribution network is contructed.
  
  ![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/c127a7d4-9cce-470e-8e94-09b9ee280ec8)

- Placement: Placement involves assigning specific locations on the chip for each gate and macro from the synthesized netlist. The goal is to optimize for various objectives, including minimizing wirelength, meeting timing constraints, and managing thermal considerations.

![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/1082057c-bb1a-4316-8c2f-32fa46f81bb3)


- Clock Tree Synthesis: Clock tree synthesis (CTS) is a crucial step in ensuring that the clock signals reach all parts of the chip with minimal skew and jitter. It involves the generation of a hierarchical tree structure to distribute the clock signals uniformly and meet timing requirements.

![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/78add544-29d3-4e18-924c-0ac60c3b8651)


- Routing: After placement and CTS, routing is performed to create the physical wires (metal traces) that connect all the components on the chip. This process adheres to design rules and timing constraints.

![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/c7d898ed-afac-48b4-a059-f1f61a6a4ab3)

- Signoff: The Signoff stage encompasses a series of verification and validation steps


## Introduction to OpenLANE and Strive chipsets

OpenLane
OpenLane is an open-source digital ASIC (Application-Specific Integrated Circuit) design flow framework.esigned to automate the process of designing and fabricating digital integrated circuits. OpenLane aims to make custom ASIC design more accessible to a broader range of engineers and researchers.


striVe is a family of open souces tools like open PDK, open EDA ,open RTL.

![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/97399b2b-2408-4fa0-b2d8-81b7967854d7)



**OpenLANE ASIC  flow**

![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/ba36bb0a-b15f-4e2e-ab8a-c8f923da64ee)


- Synthesis exploration is a process in digital integrated circuit  design where designers explore various design options and configurations during the synthesis stage. The goal is to find the best combination of logic optimizations, architecture choices, and design constraints to meet the desired performance, power consumption, and area targets for the design. 

- Design exploration refers to the process of investigating and evaluating various design alternatives, configurations, and choices to optimize a system or product's performance, functionality, cost, or other key attributes.

- Regression testing uses design exploration utility on approximately 70 designs and compares the results with the best known ones.

  ![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/4d0445ba-0390-498a-9c7a-4bfbc10c70ca)

- Design for Test (DFT) is a set of engineering techniques and practices employed during the design of integrated circuits and electronic systems to facilitate and improve the testing and verification process. The main objective of DFT is to make it easier to detect and diagnose faults, defects, or issues within the IC or system.

- Physical implementation  refers to the process of transforming a high-level logical description of an IC into a physical layout that can be fabricated. Process involves Floorplanning, Placement, Clock Tree Synthesis, Routing etc

- Logic Equivalence Check  done by using yosys. Every time the netlist is modified ,verification must be performed. CTS modifies the netlist, post placement optimizations modifies the netlist

- Antenna Rules ViolationsWhen ametal wire segment is fabricated ,it can act as an antenna. Reactive ion etching causes charges to accumulate on the wire due to which transistor gates are damaged during fabrication. Solutions are bridging attaches a higher layer intermediary or adding  antenna diode cell to leak away charges

- Static Timing Analysis RC Extraction : DEF2SPEF and 
- Static Timing Analysis RC Extraction : DEF2SPEF and


## Getting Familiar with the Open Source EDA Tools

Tool we will be  working on pdk variant called sky130_fd_sc_hd

- sky130 : is the process name
- fd : skywater foundary
- sc : standard cell
- hd(high density) : variant of pdk

**Design Preperation step**
First we go the the working directory 
```
cd Desktop/work/tools/
cd openlane_working_dir/
cd openlane
```
Now when we  type the ```docker``` command a shell opens .
In the shell we type ```./flow.tcl -interactive```


![1](https://github.com/vaishbv/pes_pd/assets/79531808/c09bb69c-ec71-406b-b337-340701cb5fbd)



Then we type ```package require openlane 0.9``` to import all the packages 

![2](https://github.com/vaishbv/pes_pd/assets/79531808/57a0137f-f71c-4215-aef0-fb9d3c65c275)



Now for the design setup stage, we will be working on picorv32a design.

```
prep -design picorv32a
```
![8](https://github.com/vaishbv/pes_pd/assets/79531808/226968ba-f1d2-4bcb-915f-93316a8e2ae1)



After preparing the design, we can see that a new 'runs' folder is created.

![4](https://github.com/vaishbv/pes_pd/assets/79531808/3970283a-98ec-475a-9c8b-496581a6644c)



Now we synthesis the design
```
run_synthesis
```

![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/ed9950fb-d0c8-4704-bb09-9ab5025ecf78)

Synthesized 

![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/6b1fc802-218d-4b66-b152-6ea447aa03df)


Flop ratio = 1613/14876 = 0.108

10.8% of the cells in our design are flip flops

Netlist is generated in the runs folder 

![5](https://github.com/vaishbv/pes_pd/assets/79531808/bf5a32e9-7db5-4479-9c72-be6823d07a68)
![6](https://github.com/vaishbv/pes_pd/assets/79531808/9b538986-c431-40ce-a49a-e52a217ff89e)
![7](https://github.com/vaishbv/pes_pd/assets/79531808/05e37000-33dd-4452-b623-f1dd4bd6a15b)

# Day 2
## Chip Floor Planning Considerations

**Utilization Factor and Aspect Ratio**

![image](https://github.com/AniruddhaN2203/pes_pd/assets/142299140/a1812369-af71-48c4-860c-f76af506400e)
- We consider a simple netlist with a Launch and Capture Flop. It also has an AND and OR gate.
- We then convert it into squares since we need appropriate dimensions

![image](https://github.com/AniruddhaN2203/pes_pd/assets/142299140/f7148664-ea48-420e-92a6-b5ef5ebc30fd)
- Let us consider the areas of the gates and Flops as 1 sq unit

![image](https://github.com/AniruddhaN2203/pes_pd/assets/142299140/6b3f5692-c22e-4c21-857d-d689adf834f0)
- Clubbing them together we get an area of 4 sq units

- The 'core' section of a chip is where the fundamental logic design is placed.
- The 'die' area contains the core and is a small semiconductor are on which the fundamental circuit is fabricated.

![image](https://github.com/AniruddhaN2203/pes_pd/assets/142299140/b2c4dbb6-2b0c-477c-9696-a5fe90c282ef)
- Now we put the netlist in the 'core' area and check the utilization.
- Here
```
Utilization Factor = Area Occupied by the Netlist/Total Area of the Core
```
- As we can see here, there is 100% utilization and ```Utilization Factor = 1```.
- In practical scenarios we don't go for such a high utilization factor.
- The 'Aspect Ratio = Height/Width = 1'.

**Concept of Pre Placed Cells**

![image](https://github.com/AniruddhaN2203/pes_pd/assets/142299140/ce3ce1b6-578f-4910-924c-d712f174e809)
- We take the above combinational logic as an example

![image](https://github.com/AniruddhaN2203/pes_pd/assets/142299140/a9cd2668-26b2-41ec-9c04-cb5b1d85f2a5)
- We split the circuit into two parts, block 1 and block 2 as shown above

![image](https://github.com/AniruddhaN2203/pes_pd/assets/142299140/6a468eae-b0f1-4eb7-ad74-0ba8ba42626d)
- We extend the I/O pins and black box the boxes.
- Now we separate the boxes and the get their respective I/O ports.
- The use of doing this is that the users can use the blocks multiple times and form the required final circuit with ease.
- They only need to implement the design once and it can be reused.
- These kind of IPs have user defined locations and are placed in the chip before automated placement and routing takes place. These are called pre-placed cells.

**Surrounding Pre-Placed Cells with Decoupling Capacitors**

![image](https://github.com/AniruddhaN2203/pes_pd/assets/142299140/10b42d05-a8a2-4a44-bd0c-08102b608e38)

- Huge capacitor filled with charge. The equivalent voltage across the capacitor is similar to what the power supply produces.
- We add the capacitor in parallel to the circuit.
- Everytime the circuit switches it draws current from the decoupling capacitor, whereas the outer network with the power supply and other componets is used to re-charge the capacitor

**Pin Placement**
- In pin placemnt step we use the HDL netlist to determine where a specific pin should be placed in the circuit.
- We join the common pins and try to keep the connections as effecient as possible.
- Pins are placed in the Die area.

**Steps to run FLoorplan using OpenLANE**
- To view floorplan we type
```
run_floorplan
```
in the OpenLANE shell.

![1](https://github.com/vaishbv/pes_pd/assets/79531808/dd441eda-bea5-4b73-a106-41c54d82b74d)


- To open the Floorplan we go to the required directory that is
```
vsduser@vsdsquadron:~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/11-09_15-36/results/floorplan
```
using the ```cd``` command.

- Then we type the command:
```
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```

- The following layout is displayed

![image](https://github.com/AniruddhaN2203/pes_pd/assets/142299140/c3eeadc7-821f-45ce-b077-7702623e25bf)
- We can press 's' and then 'v' to align the design to the center of the screen.

- We can right click on the mouse and pess 'z' to zoom into a desired part.

![image](https://github.com/AniruddhaN2203/pes_pd/assets/142299140/3d251ffe-c125-41be-a96e-00f2391b89fb)
- We can see here that the I/O ports are equidistant

![image](https://github.com/AniruddhaN2203/pes_pd/assets/142299140/2b79ae12-8c15-44e1-b800-48981f3fc442)
- We can check the details of the ports as follows
  - Hover over a port with your crosshair and press 's' on your keyboard
  - Now open the tkcon command window and type ```what```.
  - This will show you the details of the selected port.

![image](https://github.com/AniruddhaN2203/pes_pd/assets/142299140/2ee6d6b3-a7e9-415b-b2dd-1271d16dac2c)
- If we zoom in a little more, we can see the tap cells.
- They are present to prevent latch up conditions which occur in the CMOS devices

![image](https://github.com/AniruddhaN2203/pes_pd/assets/142299140/7e726389-8a96-4572-b6a7-59d5eb0d821a)
- These are the standard cells that are used in the design

## Library Binding and Placement

**Netlist Binding and Initial Place Design**

![image](https://github.com/AniruddhaN2203/pes_pd/assets/142299140/9fbd1dd6-34b3-4b38-b92a-c56efe08311f)
- In real life, the logic gates and cells do not have shapes, but are present in the form of rectangles and squares.
- Hence they have dimensions to them and the space where they are placed must be utilized carefully
- The above picture shows an example of a library.
- Library consists of various kinds of cells which have different shapes and sizes, flavours and different timing information.

![image](https://github.com/AniruddhaN2203/pes_pd/assets/142299140/adc000d3-4076-4c74-b6d3-96e3e10f311f)
- The components of the netlist are placed in the core area.
- They are placed according to the convenience of distance from the pins.
- When sending signal from FF1 to FF2, according to the circuit requirements, there has to be a very fast propogation of signals. Hence, they are placed very close and buffers are added since there is a small delay for the signal from the pin to reach FF1. The buffers maintain signal integrity

**Viewing the Placement**
- To view the placement we type
```
run_placement
```
in the OpenLANE shell.

![image](https://github.com/AniruddhaN2203/pes_pd/assets/142299140/56832e08-c84e-4c78-8a24-73e1b9bdb05f)
- This is the result displayed. As we can see the '/picorv32a.placement.def' file is read.

![2](https://github.com/vaishbv/pes_pd/assets/79531808/da01891d-37c6-4902-9975-57b2680e563c)

- We move one directory up from the 'floorplan' folder using
```
cd ../placement/
```
- To view the placement design we use the command
```
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def
```

![image](https://github.com/AniruddhaN2203/pes_pd/assets/142299140/313098df-673a-466d-8cce-169a9932fcce)
- The above is displayed.
- All these standard cells were present at the initial layout of the floorplan.

![image](https://github.com/AniruddhaN2203/pes_pd/assets/142299140/2a7cce9b-9550-4eaf-922e-8a64306f05ac)
- If we zoom in we can see the placement of the standard cells in the standard cell rows.

## Cell Design and Characterization Flow

**Cell Design Flow**
- Inputs -> Process design kits(PDKs) : DRC and LVS rules, SPICE models, library and user-defined specs.
- Design Steps -> Circuit Design, Layout Design(Euler Path and Stick Diagram), Characterization.
- Outputs -> CDL(Circuit Description Language), GDSII, LEF, extracted spice netlist(.cir)

**Characterization Flow**
- This is for an inverter.
1) Read the model files.
2) Read the extracted SPICE netlist.
3) Recognize the behaviour of the buffer.
4) Attaching the necessary power sources
5) Apply the stimulus, which is the input signal to the circuit.
6) Read the sub-circuit of the inverter.
7) Provide necessary output capacitances.
8) Provide the necessary simulation commands

**Timing Characterization**
- slew_low_rise_thr = 20%
- slew_high_rise_thr = 80%
- slew_low_fall_thr = 20%
- slew_high_fall_thr = 80%
- in_rise_thr = 50%
- in_fall_thr = 50%
- out_rise_thr = 50%
- out_fall_thr = 50%

- Propogation delay = time(out_fall_thr) - time(in_rise_thr)

- Transition Time
  - On rise: time(slew_high_rise_thr) - time(slew_low_rise_thr)
  - On fall : time(slew_high_fall_thr) - time(slew_low_fall_thr)
