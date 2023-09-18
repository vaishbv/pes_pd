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
## DAY 3 - Design library cell using Magic Layout and ngspice characterization

## Labs for inverter ngspice simulations

IO Placer revision
PnR is a iterative flow and hence, we can make changes to the environment variables when required.
For example we can change the  pin configuration along the core from equvi distance randomly placed to someother placement.

![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/83069431-583f-4228-9bda-cb47b37015ba)


**SPICE deck creation for CMOS inverter**


To simulate standard cells  we  need to  create spice deck for our cell.
The spice deck will contain
- Component connectivity which include the substrate taps that  tunes the threshold voltage of the MOS
- Component values like values of PMOS and NMOS, Output load, Input Gate Voltage, supply voltage
- Node names which  are required to define the SPICE Netlist


![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/e70a334f-604f-42ce-81af-986009595de5)


**Switching Threshold of a CMOS Inverter**
CMOS cells have three modes of operation:

Cutoff - No inversion
Triode - Inversion but no pinchoff in channel
Saturation - Inversion and pinchoff in channel

The voltages at which the switch between the modes of operation happens is dependent on the threshold voltage of the device. Threshold voltage is a function of the W/L ratio of a device, therefore varying the W/L ratio will vary the output waveform of CMOS devices.
To enable efficient description of the varying waveforms a single parameter called switching threshold is used. Switching threshold is defined at the intersection of Vin = Vout. 


![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/28a7e92a-bbfd-4428-a2c5-748995227b16)


![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/93ea6940-f72d-4181-8d61-45bb83130fd3)


## Inception of Layout and CMOS Fabrication Process

**16 mask CMOS process**

- Substrate Selection: In the initial phase, the appropriate semiconductor substrate is chosen.
- Create an active region for transistors : to isolate the active regions for transistors SiO2 and Si3N2 deposited. Pockets created using photoresist and lithography.
- Nwell & Pwell formation : P-well formation involves photolithography and ion implantation of p-type Boron material into the p-substrate.N-well is formed similarly with n-type Phosphorus material.. Drive in 
  diffusion by placing it in a high temperature furnace.
- Gate Formationg.A polysilicon layer is deposited and photolithography techniques are applied to create NMOS and PMOS gates
- Lightly Doped Drain (LDD) formation : LDD done to avoid hot electron effect and short channel effect.
- Source & Drain Formation: Thin oxide layers are added to avoid channel effects during ion implantation.N+ and P+ implants are performed using Arsenic implantation and high-temperature annealing.
- Local Interconnect Formation: Thin screen oxide is removed through etching in HF solution.Titanium deposition through sputtering is initiated. Heat treatment results in chemical reactions, producing low-    
  resistant titanium silicon dioxide for interconnect contacts and titanium nitride for top-level connections, enabling local communication.
- Higher Level Metal Formation: To achieve suitable metal interconnects, non-planar surface topography is addressed.Chemical Mechanical Polishing (CMP) is utilized by doping silicon oxide with Boron or   
  Phosphorus to achieve surface planarization.TiN and blanket Tungsten layers are deposited and subjected to CMP.An aluminum (Al) layer is added and subjected to photolithography and CMP
- Dielectric Layer Addition: Finally, a dielectric layer, typically Si3N4, is applied to safeguard the chip.


**LAB**

Cloning repository

```
git clone https://github.com/nickson-jose/vsdstdcelldesign.git
```


command for layout
```
magic -T sky130A.tech sky130_inv.mag &
```

![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/49e9db46-f491-4b02-940e-073314e4a465)

Click on the component and type ```what``` in the tkcon window

![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/26bb651d-da09-4199-a2b9-c5ecb8253425)

**DRC Errors**

DRC errors in magic will be highlighted with white dotted lines:

![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/7e023b4b-2c28-473d-a879-029feeb593a1)

To identify DRC errors select DRC find next error:
it will be displayed on the tkcon window

![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/5a339793-7050-4f7f-94f2-9eefaa3f9073)

**Extracting to SPICE**
Command 
```
extract all
ext2spice cthresh 0 rthresh 0
```
cthresh and rthresh are used to extract all parasatic capacitances.

![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/1bbef24c-f86b-4b54-b9e5-e70fc9edd21d)


SPICE file 

![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/9665e29b-cb88-4aa6-a408-e26a520628f3)


**Spice Wrapper for Simulation**

![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/0140489c-834e-4ffe-a6e6-6e19b2cf9091)


**NGPSICE**

![11](https://github.com/vaishbv/pes_pd/assets/79531808/06189e6f-28bc-4690-8878-606b8bef5b91)


Next we type the command 
```
plot y vs time a
```

**WAVEFORM**

![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/5b1f8fb1-24b1-4ae9-bb40-cd9ceffded75)


## Sky130 Tech File Labs

**Introduction to Magic tool options and DRC rules**

(refer:http://opencircuitdesign.com/magic/)
Magic is a venerable VLSI layout tool, written in the 1980's at Berkeley by John Ousterhout, now famous primarily for writing the scripting interpreter language Tcl. Due largely in part to its liberal Berkeley open-source license, magic has remained popular with universities and small companies. The open-source license has allowed VLSI engineers with a bent toward programming to implement clever ideas and help magic stay abreast of fabrication technology. However, it is the well thought-out core algorithms which lend to magic the greatest part of its popularity. Magic is widely cited as being the easiest tool to use for circuit layout, even for people who ultimately rely on commercial tools for their product design flow.


Drc section
The design rules used by Magic's design rule checker come entirely from the technology file. We'll look first at two simple kinds of rules, width and and spacing. Most of the rules in the drc section are one or the other of these kinds of rules.


SKY130 pdk
SKY130 is a mature 180nm-130nm hybrid technology developed by Cypress Semiconductor that has been used for many production parts. SKY130 is now available as a foundry technology through SkyWater Technology Foundry.

![2](https://github.com/vaishbv/pes_pd/assets/79531808/258ca773-bbc2-43e0-a67b-6cd16155b57b)


![3](https://github.com/vaishbv/pes_pd/assets/79531808/3daea58e-3efa-40d0-8197-6b9ca825cdc1)

To open the software we type
```
magic -d XR
```

![image](https://github.com/AniruddhaN2203/pes_pd/assets/142299140/8571a471-4937-4b89-8197-99c2648dfa66)
- We click 'file' and open the 'met3.mag' file.

![image](https://github.com/AniruddhaN2203/pes_pd/assets/142299140/cbec69ce-fedb-41cd-af31-0dd9750c1ed9)
![image](https://github.com/AniruddhaN2203/pes_pd/assets/142299140/b9551c50-a44b-472b-bf63-d4979c2f8caa)
- If we select an area and type ```drc why``` in the tkcon wndow, it will show us the DRC error.

![image](https://github.com/AniruddhaN2203/pes_pd/assets/142299140/53fdfab7-7d24-4638-9fb3-855589c89b83)
- To add contact cuts to metal3, first select an area using left and right click. Then hovering over the m3contact we click middle mouse button.

**Fixing DRC Errors**
- There is a DRC error in the poly.mag file in 'poly.9'.
- Open the sky130A.tech file in the editor and make the following changes

![image](https://github.com/AniruddhaN2203/pes_pd/assets/142299140/d786e946-95e0-42c8-a0bd-a83a57697e04)

![image](https://github.com/AniruddhaN2203/pes_pd/assets/142299140/4274217a-ba1b-499d-b81a-fa26b4262301)

![image](https://github.com/AniruddhaN2203/pes_pd/assets/142299140/efafb2d3-520d-4405-b1ae-731b0308ac99)
- Now open the tkcon window and type
```
load tech sky130A.tech
drc check
```

![image](https://github.com/AniruddhaN2203/pes_pd/assets/142299140/ebd7630c-128d-46ba-adb7-a30169ba7fe6)
- As we can see the error is fixed.

**DRC Error as Geometrical Construct**
- We open the nwell.mag file.

![image](https://github.com/AniruddhaN2203/pes_pd/assets/142299140/4c49e5c5-4738-449c-9df0-048d2034825d)
- We type the above commands

![image](https://github.com/AniruddhaN2203/pes_pd/assets/142299140/40097614-0dfe-4479-b85c-4418e7b43787)
- The following is displayed

**Find Missing or Incorrect Rules and Fix Them**

![image](https://github.com/AniruddhaN2203/pes_pd/assets/142299140/338b998a-9a6b-4485-8d2b-095fbbcf3d83)

![image](https://github.com/AniruddhaN2203/pes_pd/assets/142299140/cc00bc2f-31ed-45b3-96d8-a12b28697bd4)
- As we can see this is an incorrect implementation and the above rule is violated.

![image](https://github.com/AniruddhaN2203/pes_pd/assets/142299140/d689590d-b9a7-43b2-b1da-72e95a9f4dcb)

![image](https://github.com/AniruddhaN2203/pes_pd/assets/142299140/71d62451-245b-4010-8246-234727062c8b)
- We make the following changes

![image](https://github.com/AniruddhaN2203/pes_pd/assets/142299140/08900ffa-8f91-4550-be8c-375b4cb42866)
- Now we select the nwell.4 and type the following commands
```
tech load sky130A.tech
drc check
drc style drc(full)
drc check
```

![image](https://github.com/AniruddhaN2203/pes_pd/assets/142299140/dbb58cd3-4845-4d28-a22b-0228b8260cf6)
- As we can see the error still persists
- We can fix it by the following method.

![image](https://github.com/AniruddhaN2203/pes_pd/assets/142299140/4bc70446-9a20-47cd-95b9-d850e170887a)
- Select the existing nwell.4 and make a copy of it by selecting it and clicking 'c'.
- Now select a small area on the nwell.4 and add an 'nsubstratecontact' by hovering over it and clicking middle mouse button.


## DAY 4

## Pre-layout timing analysis and importance of good clock tree

## Timing Modelling using Delay Tables
Place and routing (PnR) is performed using an abstract view of the GDS files generated by Magic. The PnR tool will use the abstract view information, formally defined as LEF information, to perform interconnect routing.
From PnR POV, We have to follow certain guidelines to get standard cell set

Input and output ports must lie on the intersection of vertical and horizontal tracks
Width of the standard cell should be odd multiples of the track pitch and height should be odd multiple of vertical track pitch

**steps to convert grid info to track info**

Path to track info
```
Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/openlane/sky130fd_sc_hd
```

Now we open the track.info file


![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/08bb1a3d-c41e-405e-b960-d3fe9323b8a9)

- The tracks.info file is used during routing.
- Routes are the metal traces.

1st numeric column indicates the offset and 2nd indicates the pitch along provided direction


Setting grid values 

![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/06f300fa-2ffa-4227-b297-c08cc921b3d4)


We converge the grid definition in the layout to track definition


![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/4509cd00-6502-42e2-a477-f7573e6b51f1)



![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/c3b28612-9767-4698-a9d3-f1837e5976ed)


From the above pic, its confirmed that the pins A and Y are at the intersection of X and Y tracks. So the first condition is met.
The next requirement is that the width of the cell should be the odd multiple of xpitch which is '0.46' as seen in the tracks.info file.


**Convert Magic Layout to Standard Cell LEF**

To generate the cell LEF file from Magic first we save the modified layout and then we open the file and extract LEF
we type the command in the tkcon window

```
save sky130_vsdinv.mag
```

Now in the terminal we type 

```
 magic -T sky130A.tech sky130_vsdinv.mag
lef write
```


![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/74957701-02f0-40ca-b113-167915469817)



**Steps to Include New Cell in Synthesis**

We copy the lef file created and the sky130 library that to the src folder of picorv32a folder.

![4](https://github.com/vaishbv/pes_pd/assets/79531808/301bf668-e7f4-433a-82de-b4c23cb2eccf)

![5](https://github.com/vaishbv/pes_pd/assets/79531808/a8d5ff2b-8a75-42ed-b622-b5d8d357cb98)




Modify config file to include the libraries and lef file 

![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/2329addb-9fba-4780-a605-de8842004aab)


Next in OpenLANE we retrieve the 0.9 package.

We type the followig commands 
```
prep -design picorv32a -tag 18-09_05-15 -overwrite
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
```

![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/b4b6cfa1-9dbb-4998-9278-c16495c8c81a)


![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/d2204a1e-8391-4894-b838-034d35064f2d)


Now we ```run_synthesis```


![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/b531af6c-3bb5-407c-b6eb-3321fe29dcd9)


**steps to configure synthesis settings to fix slack and include vsdinv**

Slack violations refer to timing violations in digital designs. These violations occur when the signal arrives at its destination too early or too late, violating the specified setup or hold time constraints. 

When referring to pre clock tree synthesis STA analysis we are mainly concerned with setup timing in regards to a launch clock. STA will report problems such as worst negative slack (WNS) and total negative slack (TNS). These refer to the worst path delay and total path delay in regards to our setup timing restraint. Fixing slack violations can be debugged through performing STA analysis with OpenSTA, which is integrated in the OpenLANE tool.
The desired value of slack is above or equal to 0. 

Ways to fix slack

1.Changing  synthesis strategy in OpenLANE
 - Enalbed CELL_SIZING
 - Enabled SYNTH_STRATEGY with parameter as DELAY 1

![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/72a18623-aee6-4598-a80e-c8259752978e)

Slack still isnt in the required range.

The sdc file used is my_base.sdc defined in pre_sta.conf using the command sta pre_sta.conf

![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/f57ba8a6-fc74-455b-b323-017aeb6e46a6)

The delay is high when the fanout is high. Therefore we can re-run synthesis by changing the value of SYNTH_MAX_FANOUT variable

2.Enable cell buffering

3.Perform manual cell replacement on our WNS path with the OpenSTA tool
Net is driving most outputs and replace the driver cell with larger form of its own kind

![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/de2cca82-aa2c-4d6d-8e1a-52864c2bba9e)

4.Optimize the fanout value with OpenLANE tool

Now since synthesised the core using our vsdinv cell too and as it got successfully synthesized. We go ahead with the floorplan 

```
init_floorplan
run_placement
```

![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/5ac80fb7-d7d2-46bb-871e-9e27249eb3ab)


On zooming in 

![sky130](https://github.com/vaishbv/pes_pd/assets/79531808/0e3f6413-d1b9-4b2c-8bce-c6d9fe9a3401)



**Introduction to delay tables**

Delay tables, also known as delay models or delay tables, are essential components in digital circuit design and analysis. They provide a way to model and understand the propagation delays of logic gates and interconnects within a digital integrated circuit (IC). These tables play a crucial role in ensuring that the circuit meets its timing requirements, such as setup and hold times, and they are fundamental to the design of synchronous digital systems. Here's an introduction to delay tables:

1. Purpose of Delay Tables:

Delay tables are used to represent the delays encountered by signals as they pass through various components of a digital circuit. The primary purposes of delay tables are as follows:

Timing Analysis: They are essential for performing timing analysis, ensuring that signals meet their timing constraints, and identifying potential violations.

Synchronization: They help in synchronizing different parts of a digital system to ensure that data is sampled or latched correctly.

Power Estimation: Delay tables are used for estimating power consumption in digital circuits since power dissipation is directly related to signal transitions.

2. Components of Delay Tables:

Delay tables typically include the following components:

Input Conditions: These conditions specify the input signal values or transitions that trigger the delay calculation. Inputs can include input signal values, load conditions, and transition times.

Gate Delays: Delay tables include information about the propagation delays of various logic gates, such as AND, OR, NAND, NOR, XOR, and others. These delays depend on the gate's technology, fan-out, and input conditions.

Interconnect Delays: They account for the delays introduced by the wires and routing between logic gates. Interconnect delays depend on the physical characteristics of the wires, including length, resistance, and capacitance.

Output Loads: The output load conditions specify the capacitive load that the gate must drive, which affects the output delay.



## Timing analysis with ideal clocks using openSTA


**Setup timing analysis and introduction to flip-flop setup time**

Setup Timing Analysis:

Setup timing analysis is a critical aspect of digital circuit design and verification. It focuses on ensuring that data signals meet the required setup time constraints at the inputs of sequential elements (e.g., flip-flops) in a digital system. The primary goal of setup timing analysis is to ensure that data is stable and valid before it is clocked into a flip-flop or other storage elements.


Introduction to Flip-Flop Setup Time:

The setup time (Ts) for a flip-flop is a critical parameter that determines when a data input signal must be stable before the arrival of the clock edge to guarantee proper data capture. It is defined as the minimum amount of time the data input must be held at a valid logic level before the active clock edge (e.g., rising edge) for reliable storage.

![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/51a27aca-0064-4ea9-844f-e0201e4ccfbb)



**Introduction to clock jitter and uncertainty**

Clock Jitter:

Clock jitter refers to the short-term variations or fluctuations in the timing of a clock signal's edges. It is a critical parameter in digital systems and communication systems, as it can affect the overall system's performance, especially in high-speed or sensitive applications. Clock jitter can be caused by various factors and can manifest as random or deterministic variations in the clock signal's timing. 

Clock Uncertainty:

Clock uncertainty, also known as clock skew, is related to the variation in the arrival times of clock signals at different points within a digital system. It is distinct from clock jitter but can also impact system performance and timing. Clock uncertainty can arise due to factors such as clock distribution network delays, routing delays, and variations in clock path lengths.




**Clock Tree Synthesis**

After all the above steps of fixing slack violations. A a mapped.v file is generated in synthesis results. Therefore we write this netlist using write_verilog and replace the openlane generated mapped file ie., picorv32a.synthesis.v

now in the openlane flow, continue with 
```
run_flooorplan 
run_placement 
run_cts
```

## Clock Tree Synthesis TritonCTS and Signal Integrity


**Post CTS- STA Analysis**

OpenSTA is an open-source static timing analysis tool that is commonly used in digital circuit design. STA is a critical step in the design and verification of digital circuits, ensuring that the circuit meets its timing requirements. OpenSTA is part of the larger OpenROAD open-source RTL-to-GDSII  flow, which provides a complete ASIC design environment.

![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/e456c956-fe9b-4429-9aca-d885cbb7a6e9)


- Launch OpenRoad.
- Read lef file from tmp folder of runs
- Read def file from results of cts
- Create and save the .db  file =
- Load the .db file = 
- Read the cts generated verilog file
- read both the minimum and maximum liberty files.
- set the clocks.
- Generate timing reports

```
read_lef /openLANE_flow/designs/picorv32a/runs/18-09_06-26/tmp/merged.lef
read_def /openLANE_flow/designs/picorv32a/runs/18-09_06-26/results/cts/picorv32a.cts.def
write_db pico_cts.db
read_db pico_cts.db
read_verilog /openLANE_flow/designs/picorv32a/runs/16-09_19-58/results/synthesis/picorv32a.synthesis_cts.v
read_liberty -max $::env(LIB_SLOWEST)
read_liberty -max $::env(LIB_FASTEST)
set_propagated_clock [all_clocks]
report_checks -path_delay min_max -format full_clock_expanded -digits 4
```


![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/9479e672-0111-4854-98b8-dfa930bb06af)


The timing results wont meet our expectations due to the utilization of minimum and maximum library files which OpenRoad does not currently support for multi-corner optimization. Hence we do it using only typical corner lib


![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/da53d6a0-6f3b-419c-9fcb-f1927de18944)


We perform it again for a more accurate result


![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/eccaea84-8690-4d00-aa27-cdb1da5f736b)


`Final report 

![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/efc58ed7-8ef6-490b-9bbe-1c467897539f)


## DAY 5


## Power Distribution Network and Routing

Global and detailed routing are two essential steps in the design and manufacturing of integrated circuits (ICs), such as microchips used in electronic devices. These steps are part of the electronic design automation (EDA) process for creating semiconductor devices. 
After generating our clock tree network  we  generate the power distribution network gen_pdn using  OpenLANE:

The PDN  will create:

Power ring global for the entire core
Power halo local to any preplaced cells
Power straps to bring power into the center of the chip
Power rails for the standard cells

```gen_pdn```

![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/f5bb5b94-acfe-438e-a4d8-0d0287641042)


![image](https://github.com/Anirudh-Ravi123/pes_pd/assets/142154804/1b6bb90d-cdf3-466c-9ab1-e8e68e43a8c3)


to run the rounting we type ```run_routing```

## SPEF Extraction

After routing has been completed interconnect parasitics can be extracted to perform sign-off post-route STA analysis. The parasitics are extracted into a SPEF file. The SPEF extractor is not included within OpenLANE as of now.
```
cd ~/Desktop/work/tools/SPEFEXTRACTOR
python3 main.py <path to merged.lef in tmp> <path to def in routing>
```
The SPEF File will be generated in the location where def file is present
