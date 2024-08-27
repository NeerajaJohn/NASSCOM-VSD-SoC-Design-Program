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


      </details>
      
  -   <details>
      <summary><strong>SKY_L3 - Introduction to OpenLANE and Strive chipsets</strong></summary>
 
      **Notes: -**

      - k
      - k
    
      </details>
      
  -   <details>  
      <summary><strong>SKY_L4 - Introduction to OpenLANE detailed ASIC design flow</strong></summary>
 
      **Notes: -**
      - k
      - k
    
      </details>
      
  </details>

- <details>
  <summary><strong>SKY130_D1_SK3 - Get familiar with open-source EDA tools</strong></summary>

  - k
  - k

  </details>
