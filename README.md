# NASSCOM VSD Soc Design Program
> This repo includes notes from lectures and discussions and visual and textual documentation of Lab exercises covered in the 14 day workshop

#### Day 1: Sky130 Day 1 - Inception of open-source EDA, OpenLANE and Sky130 PDK

- <details>
  <summary><strong>SKY130_D1_SK1 - How to talk to computers</strong></summary>
  
   - <details>
      <summary><strong>SKY_L1 - Introduction to QFN-48 Package, chip, pads, core, die and IPs</strong></summary>
      
      **Notes:**
      
     All embedded boards contain processor chips. The black chip is actually a package, with the actual chip located inside this package. The package shown is a QFN (Quad Flat No-leads) 48 package. The actual chip pins are connected to the package pins using wire bonds.

     
      
      <p float="left">
        <img src="https://github.com/user-attachments/assets/d0c9bc5c-57cc-4e6c-afd5-6a61ba2dacdb" alt="Alt text" width="300" /> 
        <img src="https://github.com/user-attachments/assets/474b32b6-d601-4ae0-bc1c-09de1641b96b" alt="Alt text" width="300" /> 
      </p>      
       Upon   opening the real chip, we can see the pads that connect the pins to the outside. Any signal entering or exiting the chip does so through these pads. Then we have the core, which contains all the digital logic. The die comprises both the core and the pads together.
       Chip Internals : Inside the core, we have MACROS(SoC, GPIO Banks, SPIs) and Foundry IPs(like PLL, SDCs, DAC, SRAM)
      
      <p float="left">
        
        <img src="https://github.com/user-attachments/assets/f9cf1c1d-7253-4e8e-9678-46ff642253c3" alt="Alt text" width="300" />
        <img src="https://github.com/user-attachments/assets/c64c4eb0-0e0c-48a0-9947-4e44fa55f189" alt="Alt text" width="300" />
      </p> 
      
   - <details>
      <summary><strong>SKY_L2 - Introduction to RISC-V</strong></summary>
     
      **Notes: -**
      RISC- V ISA can be described most abstractly as the language or computer or the way in which we talk to the computer.
      If we have a C program, and it needs to be run on a particular chip layout, the entire flow of processing can be represented as below:
     
      C Program ----> Assembly Language(RISC V) ---> Machine Language(Binary form) ----> The bits get executed on the chip layout
 
      Another interface that needs to be represented between RISC V and the layout is the Hardware Description Language. The particular RISC V specifications need to be described or implented using some RTL(example implementation of picorv32 cpu core shown in image). Then follows the RTL to layout or RTL to GDSII flow. 
      ![image](https://github.com/user-attachments/assets/2886adc8-805c-4041-aa14-9df273cdfbcd)

   - <details>
      <summary><strong>SKY_L3 - From Software Applications to Hardware</strong></summary>
     
      **Notes: -**
      The Applications that we use in our computer is actually run on the chip hardware present inside. The applications (written in any language like java, c++) enters into a system software and the software converts the program/app into binary language form. The various levels/layers of systwm software in this flow is Operating System, Compiler and Assembler.
     Apart from the other jobs of OS( like Handling IO operations, Allocatiing memory etc), the majot job of OS is to compile and convert to assembly language and finally to binary form to be understood by the machine.
      ![image](https://github.com/user-attachments/assets/06d5d18f-d225-419a-9981-d14e98f7a1e1)
     An example flow is as below:
     
     Any C/C++/VB/JAVA function --> respective language compiler --> converted into hardware based instruction set--> assembler --> hexa representation of instructions(binary form. .exe file) --> enter chip--> hardware responds as per incoming bitstream.
 
     The syntax of the instruction set at compiler output is dependent on hardware architecture. E.g., for x86, ARM, RISC V types of hardware, the instruction set will also be in x86, ARM, RISC V format respectively. The final output binary pattern decides what should be the hardware should be doing. 
     ![image](https://github.com/user-attachments/assets/5de5d1bb-0d69-4bbb-8be5-9e1a19044727)
     An example of a C input program compiled into instructions is given below:
     ![image](https://github.com/user-attachments/assets/1b83de11-85f1-446d-a577-dcc865f9807a)
     The instruction set acts as an abstract interface between the C language function and the hardware. So we can say that these instruction set represents the architecture of the hardware, because it decides how the C function should interact with the hardware. So it is called the Instuction Set Architecture.
     ![image](https://github.com/user-attachments/assets/5689de20-b96b-4032-8b65-a6696927a8f6)
Another important interface between Functon and hardware is the RTL language. The output of assembler for each instruction is a binary pattern(a pattern means ADD, another pattern for Multiply. We need to build an RTL description of a hardware that will understand each binary pattern. This way of describing the hardware is called RTL implementation of the Instruction Set. This RTL is synthesized into netlist. i.e., High level RTL is converted into gates and their connection. Then follows the physical implementation of netlist. 
     ![image](https://github.com/user-attachments/assets/aa8c4d00-5214-4412-9d40-e58c2643e9c8)
   

  </details>

- <details>
  <summary><strong>SKY130_D1_SK2 - SoC design and OpenLANE</strong></summary>

  -   <details>
      <summary><strong>SKY_L1 - Introduction to all components of open-source digital asic design</strong></summary>
    
      **Notes: -**
      
      Designing ASICS requires RTL IPs, EDA Tools and PDK Kits. 100% open source ASIC design is possible due to open source RTL designs(librecores,.org, opencores.org, github.com) and EDA Tools(Qflow, OpenROAD, OpenLANE) and opensource PDK(Foss 130nm PDK)
      
      **PDKs: -**
      In earlier days, design of IC was tightly with manufacturing processeces available within each company. Later, the design was seperated from technology leading to structured design methodolgy based on λ - based rules. This gave way to Pure Lay Fabs and Fabless design companies. The interface between designers and the FAB became a set of files called PDK(Process Design Kits)
      
      PDKs include collection of files used to model a fabrication process for the EDA tools used an IC.
        - Process design Rules: DRC, LVS, PEX
        - Device Models
        - Digital Standard Cell Libraries
        - I/O Libraries ...
   
      Google and skywater together agreed to open source the PDK for the 130nm process by skywater. AS a result, in June 2020, Google released the first open sourced PDK in the market: FOSS 130nm Production PDK. 130 nm process is still relevant because of its application in many processes. Intel P4EE used 130nm process. 

      </details>
      
  -   <details>
      <summary><strong>SKY_L2 - Simplified RTL2GDS flow</strong></summary>
    
      **Notes: -**
      The major steps in RTL to GDS flow is shown below:
      ![image](https://github.com/user-attachments/assets/7e84c208-90fd-46aa-a0db-ef8b64923b25)
      1. Synthesis: Converts RTL to circuit using components from Standard Cell Library. Resulting file is gatelevel netlist
      2. Floor and Power planning:
         Different for macros and chips
          - Chip FLoor planning: partition thr chip die between different system buliding blocks and place the I/O pads
          - Macros FLoor planning:  decide macro dimensions, pin locations, row definitions etc
            
         In power planning, the power network is constructed, through horizontal and vertical rings to reduce resistance and address electromagnetition problem
      4. Placement : Place gatelevel netlist cells on rows such as to reduce interconnect length
   
         Global placement: optimal position is found for all cells, not necesssarily on rule
         
         Detailed placement: obeys rules
         
      6. Clock Tree Synthesis: Create clock distribution network to deliver clock to all sequential elements, with minimum skew and good shape(H tree, X tree etc)
      7. Routing: Signal routing develops patterns of horizontal and vertical metal patterns to connect different cells(PDK defines features of the nets). Uses divide and conquer method for forming the routing grid. GLobal routing generates routing guides, followed by detailed routing t implement actual routing.
      8. SignOff: Includes physical verification( by design rule checking and Layout vs schematic verification) and timing verification(Static Timing Analysis)

      </details>
      
  -   <details>
      <summary><strong>SKY_L3 - Introduction to OpenLANE and Strive chipsets</strong></summary>
 
      **Notes: -**

      With release of opensource PDK, e-fabless decided to create a reference opensource ASIC implementation methodology and flow called OPENLANE. It comes with APACHE 2.0 and is available in github.
 
      OpenLANE started as a True open source Tape out experiment. At e Fabless, there is a family of SoCs called striVe with open PDK, open EDA and open RTL (open Everything). Example members with various features is given below.
      
      ![image](https://github.com/user-attachments/assets/e9cb9eaf-f610-45bc-a0cf-657d92f0b96d)

      Main goal of openLANE is to produce a clean(no LVS, DRS violayions) GDSII with no human intervention. It is tuned for SkyWater 130nm Open PDK. It is containerilzed(functional out of the box). It can be used to harden macros and chips(to generate final layout).

      It has two modes of operations : Autonomous or interactive. OpenLANE has design space exporation to find the best set of flow configurations. 

      OpenLANE comes with many design examples. 
    
      </details>
      
  -   <details>  
      <summary><strong>SKY_L4 - Introduction to OpenLANE detailed ASIC design flow</strong></summary>
 
      **Notes: -**
      OpenLANE ASIC flow:
       
      The design flow starts at Design RTL and ends at GDSII file taking the SKY130 PDK as input function.
 
      OpenLANE is based on several opensource projects like OpenROAD, Magic VLSI Layout Tool, K Layout, Fault, Yosysy, QFlow and ABC.
      ![image](https://github.com/user-attachments/assets/aedd1fff-5d2e-4552-813c-76f6c3d32f60)
      
      The OpenLANE has Synthesis exploration to explore different strategies for best area and delay outputs. It has also more than 35 Design Exploration to get best design configuration, best result and clean layout. The design exploration utility is also used for regression testing. We run OpenLane on ~70 designs and compare the results. There is Design for Test option by Fault. 
    
      </details>
      
  </details>

- <details>
  <summary><strong>SKY130_D1_SK3 - Get familiar with open-source EDA tools</strong></summary>

  - SKY_L1 - OpenLANE Directory structure in detail
    
      ![image](https://github.com/user-attachments/assets/d710bd2e-48dd-4147-824d-fcaf0b75994c)

      ![image](https://github.com/user-attachments/assets/e71f311e-4821-4c06-9d5a-c7da11a6209c)
 
  - SKY_L2 - Design Preparation Step

    Change directory :
    ```bash
    cd Desktop/work/tools/openlane_working_dir/openlane          
    ```
    Invoke openlane
    ```bash
    ~/Desktop/work/tools/openlane_working_dir/openlane$ docker
    ```
    This will invoke the efabless openlane flow contained sub system docker. Now we use interactive flow.tcl method(or else it will run complete flow at once)
    ```bash
    bash-4.2$ ./flow.tcl -interactive
    ```
    ![image](https://github.com/user-attachments/assets/dc732b61-60a2-4ff2-bbbe-7c1c0ad61f19)
    Now we give required package and then prepare the design to have required files(like RTL src file and sdc files and config files etc). So we point to the required design file . e.g. picorv32
    ```bash
    % package require openlane 0.9

    % prep -design picorv32a
    ```
    ![image](https://github.com/user-attachments/assets/02d0821d-104e-4607-af9f-eb603eee02c9)

  - SKY_L3 - Review files after design prep and run synthesis
    ```bash
    cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs
    ```
    This contains working date's folder, inside which we can find the required files created now for the flow
    ![image](https://github.com/user-attachments/assets/c6c6cb3c-bb22-4c68-9677-099dd196ba28)
    
    
  - k

  - k



  </details>
