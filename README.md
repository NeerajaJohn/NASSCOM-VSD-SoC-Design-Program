# NASSCOM VSD Soc Design Program
> This repo includes notes from lectures and discussions and visual and textual documentation of Lab exercises covered in the 14 day workshop


### Inception of open-source EDA, OpenLANE and Sky130 PDK


- <details>
  <summary><strong>How to talk to computers</strong></summary>
  
   - <details>
      <summary><strong> Introduction to QFN-48 Package, chip, pads, core, die and IPs</strong></summary>
      
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
      <summary><strong> Introduction to RISC-V</strong></summary>
     
      **Notes: -**
      RISC- V ISA can be described most abstractly as the language or computer or the way in which we talk to the computer.
      If we have a C program, and it needs to be run on a particular chip layout, the entire flow of processing can be represented as below:
     
      C Program ----> Assembly Language(RISC V) ---> Machine Language(Binary form) ----> The bits get executed on the chip layout
 
      Another interface that needs to be represented between RISC V and the layout is the Hardware Description Language. The particular RISC V specifications need to be described or implented using some RTL(example implementation of picorv32 cpu core shown in image). Then follows the RTL to layout or RTL to GDSII flow. 
      ![image](https://github.com/user-attachments/assets/2886adc8-805c-4041-aa14-9df273cdfbcd)

   - <details>
      <summary><strong> From Software Applications to Hardware</strong></summary>
     
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
  <summary><strong>  SoC design and OpenLANE</strong></summary>

  -   <details>
      <summary><strong> Introduction to all components of open-source digital asic design</strong></summary>
    
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
      <summary><strong> Simplified RTL2GDS flow</strong></summary>
    
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
      <summary><strong> Introduction to OpenLANE and Strive chipsets</strong></summary>
 
      **Notes: -**

      With release of opensource PDK, e-fabless decided to create a reference opensource ASIC implementation methodology and flow called OPENLANE. It comes with APACHE 2.0 and is available in github.
 
      OpenLANE started as a True open source Tape out experiment. At e Fabless, there is a family of SoCs called striVe with open PDK, open EDA and open RTL (open Everything). Example members with various features is given below.
      
      ![image](https://github.com/user-attachments/assets/e9cb9eaf-f610-45bc-a0cf-657d92f0b96d)

      Main goal of openLANE is to produce a clean(no LVS, DRS violayions) GDSII with no human intervention. It is tuned for SkyWater 130nm Open PDK. It is containerilzed(functional out of the box). It can be used to harden macros and chips(to generate final layout).

      It has two modes of operations : Autonomous or interactive. OpenLANE has design space exporation to find the best set of flow configurations. 

      OpenLANE comes with many design examples. 
    
      </details>
      
  -   <details>  
      <summary><strong> Introduction to OpenLANE detailed ASIC design flow</strong></summary>
 
      **Notes: -**
      OpenLANE ASIC flow:
       
      The design flow starts at Design RTL and ends at GDSII file taking the SKY130 PDK as input function.
 
      OpenLANE is based on several opensource projects like OpenROAD, Magic VLSI Layout Tool, K Layout, Fault, Yosysy, QFlow and ABC.
      ![image](https://github.com/user-attachments/assets/aedd1fff-5d2e-4552-813c-76f6c3d32f60)
      
      The OpenLANE has Synthesis exploration to explore different strategies for best area and delay outputs. It has also more than 35 Design Exploration to get best design configuration, best result and clean layout. The design exploration utility is also used for regression testing. We run OpenLane on ~70 designs and compare the results. There is Design for Test option by Fault. 
    
      </details>
      
  </details>

- <details>
  <summary><strong> Get familiar with open-source EDA tools</strong></summary>

  -   <details>
      <summary><strong> OpenLANE Directory structure in detail</strong></summary>
    
      ![image](https://github.com/user-attachments/assets/d710bd2e-48dd-4147-824d-fcaf0b75994c)

      ![image](https://github.com/user-attachments/assets/e71f311e-4821-4c06-9d5a-c7da11a6209c)
      <details>
      
  -   <details>
      <summary><strong> Design Preparation Step</strong></summary>
  
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

      The first prep step is the **merging of .tlef file(with metal layer info) and cell .lef file(with standard cell info)**.

      
  -   <details>
      <summary><strong> Review files after design prep and run synthesis</strong></summary>
      
      ```bash
      cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs
      ```
      
      This contains working date's folder, inside which we can find the required files created now for the flow. 
    
      ![image](https://github.com/user-attachments/assets/c6c6cb3c-bb22-4c68-9677-099dd196ba28)
      
      We can use the following command in another terminal to inspect the resulting config file. The advantage of openlane is that we can change the configurations on the fly. (use q button to close files opened in terminal using less command)
      
      ```bash
      ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/28-08_20-544 less config.tcl
      ```
      Next step is synthesis
      ```bash
      run_synthesis
      ```
      ![image](https://github.com/user-attachments/assets/726f341a-2708-452b-9210-674aaf456d69)
      
  -   <details>
      <summary><strong> OpenLANE Project Git Link Description</strong></summary>
     
      All the information regarding openlane can be found in the github page: openlane efabless.
  
      Another resource is fossi dial up youtube video.
   
  -   <details>
      <summary><strong> Steps to characterize synthesis results</strong></summary>
  
      ![image](https://github.com/user-attachments/assets/a6008b60-5c73-4936-9fd7-aac138fb2e25)
  
      ```math
      No. of DFF = 1613
      ```
      ```math
      No. of cells =\ 14876
      ```
      ```math
      Flop\ ratio =\ 1613/14876
      ```    
      ```math
      = 0.108429685 = 10.84 \%
      ```
      Synthesized netlist
      ```bash
      ~~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/28-08_20-54/results/synthesis$ less picorv32a.synthesis.v
      ```
      ![image](https://github.com/user-attachments/assets/1993ede8-b712-4906-ba49-85f1279622a4)
      
      Report after synthesis:
   
      ![image](https://github.com/user-attachments/assets/3093bdf6-be3e-4d27-80e4-7b0f34cb2f4f)
  
      ```bash
      ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/28-08_20-544/reports/synthesis$ less 1-yosys_4.stat.rpt
      ```
      ![image](https://github.com/user-attachments/assets/e05cfb95-2316-4a35-8c35-2ea28c3dc2ef)
  
  
    </details>

  
###  Good floorplan vs bad floorplan and introduction to library cells


- <details>
  <summary><strong>  Chip Floor planning considerations</strong></summary>
  
   -  <details>
      <summary><strong> Utilization factor and aspect ratio</strong></summary>
     
      ![image](https://github.com/user-attachments/assets/174643f3-7a9f-4a95-8628-5d2923bad597)

   -  <details>
      <summary><strong> Concept of pre-placed cells</strong></summary>
     
      ![image](https://github.com/user-attachments/assets/a8e3e6a1-2234-409a-bf90-856f8fc719d0)
     
      ![image](https://github.com/user-attachments/assets/b03fca08-c1cf-4b38-bfc4-6fb8448669c6)
     
      ![image](https://github.com/user-attachments/assets/90cfdeeb-78da-4d2b-b95d-7e3023bfea12)


   -  <details>
      <summary><strong>  De-coupling capacitors</strong></summary>
     
      ![image](https://github.com/user-attachments/assets/52087f4f-e86d-4a65-8458-28b90f547d0c)

      ![image](https://github.com/user-attachments/assets/99b494ca-4b5a-48b9-aae8-e2f94234785e)

      Whenever there is a switching activity, the decoupling capacitor provides some charge to the circuit. When there is no switching activity, this capacitor replenishes its charge. In chip, it is places as shown below:

      ![image](https://github.com/user-attachments/assets/f1473092-d82b-4d50-aaf9-7574475afc36)

      Decoupling helps to avoid power loss and cross talk

     
   -  <details>
      <summary><strong>  Power planning</strong></summary>
            
      Just like a macro requires decoupling capacitor to provide for sudden voltage requirement as well as discharge scenarios, the whole chip with lots of macros require adjacent Vdd and Vss to maintain the signal shape from driver to load. Avoiding ground bounce and voltage droop outside noise margin is difficult with single tap source.
       
      The proble of single source and a possible solution is given below:
 
           
      <p float="left">       
        <img src="https://github.com/user-attachments/assets/2bb2aeea-c81c-4369-bf0f-14eff222a0d5" alt="Alt text" width="300" />        
        <img src="https://github.com/user-attachments/assets/e5e1d1f3-e5b1-436c-bab0-f40532fd2806" alt="Alt text" width="300" />
     
      </p>   
   
      To place chip components near to source and ground, modern chips use power mesh fro source as well as ground so that any sudden requirement of charging or discharging can be addressed by the nearest power/ground points.

      ![image](https://github.com/user-attachments/assets/fadb62f7-22f6-40fb-bfaf-62d96c6af4ed)

     
  -   <details>
      <summary><strong> Pin placement and logical cell placement blockage</strong></summary>

      ![image](https://github.com/user-attachments/assets/73bd85c3-7099-4161-807b-acc22a2e2619)

      ![image](https://github.com/user-attachments/assets/16e17cff-00d2-4864-b0c1-cbca62e7e068)

  -   <details>
      <summary><strong> Steps to run floorplan using OpenLANE</strong></summary>
     
      In OpenLANE there are many switches with which we can adjust the flow directions. To see this, we need to go to configurations folder.
     
      ```bash
      vsduser@vsdquadron:~/Desktop/work/tools/openlane_working_dir/openlane/configuration$ less README.md
      ```
      
      > Here we can see variables associated with synthesis and floorplan
 
     
      ![image](https://github.com/user-attachments/assets/edd879c4-b7da-4cb7-bd7d-6be74be2eaa2)
       
      ```bash
      vsduser@vsdquadron:~/Desktop/work/tools/openlane_working_dir/openlane/configuration$ less floorplan.tcl
      ```
      > Here we can observe the floorplan default parameters
 
      ![image](https://github.com/user-attachments/assets/736a9139-6102-45af-bec7-a4e2c793315b)

      > config files in the selected design can be seen below:
     
      ![image](https://github.com/user-attachments/assets/0c2438a9-89bc-4cb3-b1da-38d546ec7c7f)
     

      > The priority precedence:
 
      - Lowest :system defaults
      - next : config.tcl
      - most : <pdk_variant>.tcl (eg. sky130A_sky130_fd_sc_hd_config.tcl)
 
      > upon opening the config.tcl
            
      ![image](https://github.com/user-attachments/assets/31ab7df5-ad8e-438f-8a17-c533de2630e2) 


      To run floor planning in OpenLANE flow,
     
      ```bash
      run_floorplan
      ```
            
      <p float="left">       
        <img src="https://github.com/user-attachments/assets/b619d4f5-31c3-4204-820a-a625acea285c" alt="Alt text" width="400" />        
        <img src="https://github.com/user-attachments/assets/4275fc04-4c28-4117-bc4d-8fd9248d1ff9" alt="Alt text" width="400" />
           
      </p> 
 
  -   <details>
      <summary><strong> Review floorplan files and steps to view floorplan</strong></summary>      
 
      To check if config.tcl precedence has taken over system defaults, we can go to logs --> floorplan

      ```bash
      vsduser@vsdquadron:~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/28-08_20-54/logs/floorplan$ less 4-ioplacer.log
      ```
 
      <p float="left">       
        <img src="https://github.com/user-attachments/assets/7e6f8b2c-127e-476d-b1b3-a59e3668f603" alt="Alt text" width="400" />        
        <img src="https://github.com/user-attachments/assets/9c480e10-1c08-4373-89c1-f6fa47819dd9" alt="Alt text" width="400" />
     
      </p> 
      
      Checking config.tcl lets us know which all parameters were included in the current flow
 
      ```bash            
      vsduser@vsdquadron:~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/28-08_20-54/results/floorplan$ less picorv
      ```
      
      ![image](https://github.com/user-attachments/assets/707c7e42-e14a-4469-abcf-7c5bd3e9b94d)

      ```bash
      vsduser@vsdquadron:~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/28-08_20-54/$ less config.tcl
      ```
      Now open the terminal where we saw reports and change folder to picorv32a --> runs --> <date_folder> --> results --> floorplan 
        
      Then open floorplan.def file.
     
      ```bash
      vsduser@vsdquadron:~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/28-08_20-54/results/floorplan$ less picorv32a.floorplan.def
      ```
      ![image](https://github.com/user-attachments/assets/0008d4f8-a16e-4ebe-9b06-e35bddc20fa4)
 
      
      ```tcl
      def means data exchange format

      In floorplan.def, it is given that 1000 design units = 1 micron.

      Die area = width * height

      = [(660685-0)/1000] * [(671405-0)/1000]

      = 660.685 * 671.405

      = 4,43,587.212425 sq. micron

      ```
            
      def file is not easy to understand. So we can use **MAGIC** tool to see the actual layout after floorplan

      ```bash
      vsduser@vsdquadron:~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/28-08_20-54/results/floorplan$ magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
      ```

      ![image](https://github.com/user-attachments/assets/41c3af5d-2cf4-44b0-bcbf-1c2b46e416a5)
  
      This opens up magic layout tool as below:
      
      ![image](https://github.com/user-attachments/assets/816661a7-55ae-4812-b336-44b79e3f3542)


     
  -   <details>
      <summary><strong>SKY_L8 - Review floorplan layout in Magic</strong></summary>
      
      We can see the floorplan. Maximize window and press s to select the entire layout. Then press v to put the design at centre
  
      Zooming in: press left click and move cursor, then right click, then press z

      <p float="left">       
        <img src="https://github.com/user-attachments/assets/cfd3d3f5-a3ae-4643-a6ed-fd28738a3d09" alt="Alt text" width="400" />        
        <img src="https://github.com/user-attachments/assets/3a849753-2a0d-44b6-9a5f-0bf4ba1d0b77" alt="Alt text" width="400" />
     
      </p> 



      We set IO mode as 1, so IO pins are placed equidistantly. We can see the details of each elements by selecting using s and typing what in tkcon window, as shown below:

      <p float="left">       
        <img src="https://github.com/user-attachments/assets/e84a4b68-efe9-4456-bb87-42f85936a6c0" alt="Alt text" width="400" />        
        <img src="https://github.com/user-attachments/assets/eb068cd3-42cd-445f-b514-c37c4f94a0ba" alt="Alt text" width="400" />
     
      </p> 
      
      There are decap cells
      
      Tap cells avoid latchup condition in CMOS devices. These are placed diagonally equidistant set by config file.

      Floorplan doesnt do standard cell placements.(clock buffer, or gate etc)

      
- <details>
  <summary><strong> Library Binding and Placement</strong></summary>

  
  
  -   <details>
      <summary><strong> Netlist binding and initial place design</strong></summary>
     
      > Assigning exact shape and size with width and height for every component in the circuit
      > 
      > All information is available in libraries(with sub libraries for delay, conditions for output, shape info etc), and in different flavours of same cell
      >
      > Placement will avoid area with pre placed cells
      >
      > Cells are placed close to IO pins and logical connectivity is maintained
      
      
      ![image](https://github.com/user-attachments/assets/816cf83d-3217-47d8-9e26-09ae3a7fdc23)

   - <details>
      <summary><strong> Optimize placement using estimated wire-length and capacitance</strong></summary>

      > Diagonally opposite placement requiremnt with longer routing can be handled by optimization
      >
      > We estimate wirelength and capacitance before routing( will check slew to see if distance between cells are reasonable)
      >
      > We insert repeaters to maintain signal integrity using buffers
      >
      > More area will be cost
      
      
      ![image](https://github.com/user-attachments/assets/1ebeeb2e-4c03-4ddf-834a-99a7b6fe38db)


   - <details>
      <summary><strong> Final placement optimization</strong></summary>

      > Abutment : connecting certain logic to reduce delay
      >
      > This is very much useful for high frequency circuit
      >
      > When buffers can't be placed in some blocked area, we have to use clocking and routing algorithms for handling criss cross connections
      >
      > We csn use different metal layers in such cases
      >
      > Based on ideal clock conditions, setup timing analysis will be done, to check if specifications are met
      
      ![image](https://github.com/user-attachments/assets/6c318cad-ac22-4e38-af2e-41e8b95ef246)

      
   - <details>
      <summary><strong>  Need for libraries and characterization</strong></summary>

      > Step of logic design :
      >
      > Synthesis :arrangement of gates and proper connection that will represent the functionality of the design
      >
      > FLoor planning : Decide area of cells
      >
      > Placement : place on chip to meet initial timing
      >
      > Clock tree synthesis : to get zero clock skew
      >
      > Routing : eg. maze routing
      >
      > Static Timing Analysis: sign off timing analysis
      >
      > In all steps, Gates or cells are common. The collection of characteristics of gates is present in library
      


   - <details>
      <summary><strong>  Congestion aware placement using RePlAce</strong></summary>

      > Placement is congestion based
      >
      > Timing is not considered now
      >
      > There are different tools for global and detailed placement
      >
      > Legaization : No overlap, Abutment, Placed in correct cell position
      >
      > Globel placement main focus is reduction of Half Parameter Wire length
      >

      To run placement in OpenLANE flow,
     
      ```bash
      run_floorplan
      ```
 
      <p float="left">       
        <img src="https://github.com/user-attachments/assets/8e9a9d60-494a-4c52-a2de-fd10a1803edf" alt="Alt text" width="400" />        
        <img src="https://github.com/user-attachments/assets/58066956-7f76-48d3-b2e1-91faaaf17914" alt="Alt text" width="400" />
     
      </p> 
      

      > Many iterations are done to converge , first Global, then detailed placement is done
      >
      > To see what happened after placement, we can use magic tool again
 
      

      ```bash
      vsduser@vsdquadron:~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/28-08_20-54/results/floorplan$ magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
      ```

      ![image](https://github.com/user-attachments/assets/126b8134-91f8-4b59-9e15-2202d439a59d)

      ![image](https://github.com/user-attachments/assets/3e9d56b6-68fd-4720-ae9d-3f6f297bddc3)

      ![image](https://github.com/user-attachments/assets/1c0b876d-22c0-4919-b817-7d64278bae4e)

      ![image](https://github.com/user-attachments/assets/33d63fa5-5547-43b5-9d28-305e3af5ffc1)

      ![image](https://github.com/user-attachments/assets/50d07b14-bf36-437c-b7fc-61f10ee37e61)

      > Floorplan doesnot create power network
      >
      > It is done post CTS
      
- <details>
  <summary><strong> Cell design and characterization flows</strong></summary>

  
   - <details>
      <summary><strong>  Inputs for cell design flow</strong></summary>

      > Standard cell: the different cells from library
      >
      > 

   - <details>
      <summary><strong>  Circuit design step</strong></summary>

      >

   - <details>
      <summary><strong>  Layout design step</strong></summary>

   - <details>
      <summary><strong>  Typical characterization flow</strong></summary>



- <details>
  <summary><strong>  General timing characterization parameters</strong></summary>
  
   - <details>
      <summary><strong> Timing threshold definitions</strong></summary>
          

   - <details>
      <summary><strong> Propagation delay and transition time</strong></summary>


### Design library cell using Magic Layout and ngspice characterization


- <details>
  <summary><strong> Labs for CMOS inverter ngspice simulations</strong></summary>
  
   - <details>
      <summary><strong> IO placer revision</strong></summary>

      > We can change the parameters for floorplan on the fly
      >
      > Reset the variable and run the flow again
      >

      > Before setting IO mode to another value(equidistant)
      
      ![image](https://github.com/user-attachments/assets/39261f78-367b-44cc-8f9a-9df62a09f3c6)

      > After setting IO Mode to 2
      >
      > We can see that in 2nd layout, IO pins are stacked at one place instead of being placed equidistantly
      ![image](https://github.com/user-attachments/assets/2bc51f52-9a80-468e-82b2-1a9c6c1f6914)

   - <details>
      <summary><strong>SPICE deck creation for CMOS inverter</strong></summary>

   - <details>
      <summary><strong> SPICE simulation lab for CMOS inverter</strong></summary>

   - <details>
      <summary><strong> Switching Threshold Vm</strong></summary>

   - <details>
      <summary><strong> Static and dynamic simulation of CMOS inverter</strong></summary>
      
   - <details>
      <summary><strong> Lab steps to git clone vsdstdcelldesign</strong></summary> 
     
      > steps to clone the magic design files from a repository
 
      ```bash
      vsduser@vsdquadron:~/Desktop/work/tools/openlane_working_dir/openlane$git clone https://github.com/nickson-jose/vsdstdcelldesign
      ```
      
      ![image](https://github.com/user-attachments/assets/766b23f6-8180-4c39-a48e-c8b1f4c7515f)

      Copy mag file from libs.tech folder to vsdstdcelldesign folder

     ```bash
     vsduser@vsdquadron:~/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic$ cp sky130A.tech /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign/
      ```
      ![image](https://github.com/user-attachments/assets/457f0894-4596-43a4-a777-56e48ab99d30)

      > Now we can open the layout of inverter cell copied from github:
      
      ```bash
      vsduser@vsdquadron:~/Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign$ magic -T sky130A.tech sky130_inv.mag &
      ```

      ![image](https://github.com/user-attachments/assets/ad94b999-2081-4ad0-a1ab-bdd438cd25c5)

      
- <details>
  <summary><strong>Inception of Layout and CMOS fabrication process</strong></summary>
  
   - <details>
      <summary><strong> Create Active regions</strong></summary>
      

   - <details>
      <summary><strong> Formation of N-well and P-well</strong></summary>

   - <details>
      <summary><strong> Formation of gate terminal</strong></summary>

   - <details>
      <summary><strong> Lightly doped drain (LDD) formation</strong></summary>

   - <details>
      <summary><strong> Source and drain formation</strong></summary>

   - <details>
      <summary><strong> Local interconnect formation</strong></summary>

   - <details>
      <summary><strong> Higher level metal formation</strong></summary>

   - <details>
      <summary><strong> Lab introduction to Sky130 basic layers layout and LEF using inverter</strong></summary>

      > nmos is highlighted
      >
      ![image](https://github.com/user-attachments/assets/73081fc0-19ca-49d7-a82e-3d89b44ab067)

      > pmos is highlighted
      >
      ![image](https://github.com/user-attachments/assets/3cf18c4c-496a-4294-b0cf-f5d195ece7f0)

      > output Y
      >
      ![image](https://github.com/user-attachments/assets/e448df10-6a41-472c-874b-5526dcbc5252)

      > polysilicon
      >
      ![image](https://github.com/user-attachments/assets/035f2378-3bf1-4ff9-b143-d5716aeba818)

      > VDD connected (VPWR)
      >
      ![image](https://github.com/user-attachments/assets/5ea748dc-3547-43a1-b44d-427555ce76ac)

      > GND
      >
      ![image](https://github.com/user-attachments/assets/ccb9aa4a-fd25-4048-9d3d-7ba9f82e5254)

      > Gates of both Transistors are connected to the input(press s 3 times)
      >
      ![image](https://github.com/user-attachments/assets/a3942c36-ae1e-400d-b0d5-4c7260bde486)

      > Layers
      >
      > First layer : local interconnect layer --> locali
      > 2nd layer : metal 1 light purple
      > 3rd layer : metal 2 pink
   - <details>
      <summary><strong>Lab steps to create std cell layout and extract spice netlist</strong></summary>   

      > Std cell definition says VDD and gnd to be in metal 1
      >
      > First make all layers
      >
      > Magic has DRC tool which checks for Design rules
      >
      > DRC = 0 shows no errors
      >
      > DRC = 4 shows errors after I deleted some part of locali
      >
      > White dots show area which has design rule problems


      <p float="left">       
        <img src="https://github.com/user-attachments/assets/9778e618-cca5-46dc-ad1b-8a3a92f895e6" alt="Alt text" width="400" />        
        <img src="https://github.com/user-attachments/assets/217107f6-93e1-49aa-8f45-d49b16237c0b" alt="Alt text" width="400" />
     
      </p>   
      > DRC error seen from tckon window
      
      ![image](https://github.com/user-attachments/assets/d06c56cd-15f5-4bfd-967c-648a8d8d9106)

      > To know logical function of the design, we need to extract the SPICE and do simulation in ngspice tool
      >
      > To extract SPICE, the following steps are done
      >
      ```bash
      #Check directory
      pwd
      #Create an extraction file
      extract all
      #Use ext file to create SPICE file for use in ngspice tool(we extract parasitic capacitance first)
      ext2spice cthresh 0 rthresh 0
      #finishing conversion
      ext2spice
      ```
      > ext file created
      
      ![image](https://github.com/user-attachments/assets/3a013083-f2a9-4b7f-b18a-d5d2ddbf75ac)
      
      > Tkcon window
      
      ![image](https://github.com/user-attachments/assets/6facc125-8407-4acb-bd6f-6a62487c1078)
      
      > ls -ltr showing newly created files in vsdstdcelldesign folder

      ![image](https://github.com/user-attachments/assets/293e29c6-f5b0-4447-9362-fa38be580b88)

      > Opening the spice file to see contents
      
      ```bash
      vim sky130_inv.spice
      ```
      ![image](https://github.com/user-attachments/assets/71808723-e248-4233-9709-debf701dadff)

      
- <details> 
  <summary><strong>Sky130 Tech File Labs</strong></summary>
  
   - <details>
      <summary><strong>Lab steps to create final SPICE deck using Sky130 tech</strong></summary>

      > In the spice file, check grid size
      
      ![image](https://github.com/user-attachments/assets/c934c606-ee7a-45b9-98c4-af20b00a7335)

     
      > To ensure scaling is proper (grid size shows the dimensions to measurement)
      >
      > we change scale to 0.01 u in the spice file and edit the file as shown below
      >
      ![image](https://github.com/user-attachments/assets/3fe46d03-6b92-4fc1-bba2-a9bec539e9bc)

      > Now we can run this file using ngspice tool(install ngspice using sudo apt install ngspice)
      ```bash
      vsduser@vsdquadron:~/Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign$ ngspice sky130_inv.spice
      ```
      
   - <details>
      <summary><strong>Lab steps to characterize inverter using sky130 model files</strong></summary>
      > To plot chara:
      ```bash
      ngspice 1 -> plot y time a
      ```
     
      ![image](https://github.com/user-attachments/assets/1edd294c-993d-45b8-98c6-ca1c28dd7fe5)

      > Generated plot
      >
      ![image](https://github.com/user-attachments/assets/1176e4b5-d691-4e55-877c-7d7e2ca98089)

      >Calculate rise time
      >
      >20% of 3.3 V = 0.66V
      >
      >80% of 3.3V = 2.64V
      >
      ![image](https://github.com/user-attachments/assets/9a91a2aa-ac0c-47a6-a7d8-eb0f9f48bb6c)

      > Rise Time = 2.25224e-09 - 2.18587e-09
      >
      > = 6.637e-11s = 66.37ps

      > Fall time calculation:
      >
      > Fall Time = 4.09731e-09 - 4.05614e-09
      >
      >           = 41.7ps
      
      ![image](https://github.com/user-attachments/assets/cd496309-9d6e-4bb9-86e8-764fc43cb814)
      > Fall cell Delay/Propagation delay: Time between input falls to 50 % and output rises to 50%
      >
      > 50% of 3.3V = 1.65V
      >
      > 2.21517e-09-2.15e-09 = 6.517e-11 = 65.17 ps
      
      ![image](https://github.com/user-attachments/assets/79a6f31a-e3f7-40f5-b457-d5d6558a462f)
      >Cell Rise Time
      >
      >Time between output falls to 50 % and input rises to 50 %
      >
      > 4.07993e-09-4.05007e-09 = 2.986e-11 = 29ps
      
      ![image](https://github.com/user-attachments/assets/f946b4f7-9793-45a5-bf45-a4e9c5861677)


      
   - <details>
      <summary><strong>Lab introduction to Magic tool options and DRC rules</strong></summary>      
      > Documentation: http://opencircuitdesign.com/magic

      > Magic technology file:
      >
      > CIF
      >
      > 
      
   - <details>
      <summary><strong> Lab introduction to Sky130 pdk's and steps to download labs</strong></summary>
     
      ```bash
      $: cd

      $: wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz

      $: tar xfz drc_tests.tgz

      $: cd drc_tests

      $: gvim .magicrc

      $: magic -d XR &

      ```
      
      <p float="left">       
        <img src="https://github.com/user-attachments/assets/3d04c81c-c330-439a-9abe-b0f1d999d368" alt="Alt text" width="400" />   
        
        <img src="https://github.com/user-attachments/assets/f1a48d7e-66c7-48b3-99c2-a33c736170e4" alt="Alt text" width="400" />
     
      </p>   
      > .magicrc file 

      ![image](https://github.com/user-attachments/assets/e7b72ebc-d13d-49dd-b4f0-5b023b7b784d)

      
       
   - <details>
      <summary><strong>  Lab introduction to Magic and steps to load Sky130 tech-rules</strong></summary>
      
      > Link for periphery rules : 
     
      [https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html](https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html#m3)  
      ![image](https://github.com/user-attachments/assets/2d141982-bb5d-4f80-82d3-fb62cbccbef2)
      > File -> open -> met3.mag
      ![image](https://github.com/user-attachments/assets/88944d1b-e17d-4c66-8ea2-de9813d50b8e)
      > using ; to type in tkcon directly from GUI
      > 
      > ; drc why (returns drc error in console window)
      
      ![image](https://github.com/user-attachments/assets/b49aa933-80a1-466a-9637-5207fae647f8)

      > Derived layers
      >
      > Fill in a large area with met3 contact
      >
      > hover over that area and type
      >
      > cif see VIA2
      >
      > shows mask with contact cuts
      >
      > feed clear to not show the cuts
      ![image](https://github.com/user-attachments/assets/3a9a4609-f5e2-4787-8f66-2e33111ebe6f)
      
   - <details>
      <summary><strong>  Lab exercise to fix poly.9 error in Sky130 tech-file</strong></summary> 
      > load poly
      
      ![image](https://github.com/user-attachments/assets/5a386c9d-bb30-43ac-9d9c-005f27d1611f)
      > Find error
      >
      > Distance should be 0.48u, but box command of area selected between poly and polynres is 0.21u, so violating the rules
      ![image](https://github.com/user-attachments/assets/3694f80a-e1bb-4ca1-9034-b8d715efdf7c)
      > Finding poly.9 in sky130A.tech file opened using vi
      >
      > vi sky130A.tech
      >
      ![image](https://github.com/user-attachments/assets/36dbf069-89cc-4b1f-bafa-d02536457f63)
      >
      >Insert new rule as shown below:
      >
      ![image](https://github.com/user-attachments/assets/d9ba37d6-a5d5-484d-9826-0a3d1913132a)
      > After editing, load tech file from tkcon
      >
      ![image](https://github.com/user-attachments/assets/2ae607ba-4ae5-4050-98ed-3781d259d55a)
      ![image](https://github.com/user-attachments/assets/d6cc7354-fb7b-4f59-b6d8-a92c857018df)
      
      
  
   - <details>
      <summary><strong> Lab exercise to implement poly resistor spacing to diff and tap</strong></summary>
     
      > copy resistors
      > 
      

      > finding drc error after editing rules
      >
      ![image](https://github.com/user-attachments/assets/aad590e9-9730-43de-ac0c-b3c65cfe04d8)
      > All errors are not accounted for
      > so edit tech file and load again
      >
      ![image](https://github.com/user-attachments/assets/aef32a61-eeed-4efc-bb74-8fc7565fc42c)
      ![image](https://github.com/user-attachments/assets/5d21feba-675a-4fab-805b-3565e87c4120)

   - <details>
      <summary><strong>  Lab challenge exercise to describe DRC error as geometrical construct</strong></summary>
      
      > Figure out how to describe the area as a geometrical constuct, similar to Calibre error check
      >
      > After all the boolean operators are applied in sequence, what ever leftover is a DRC error
      >
      > Way to tie DRC section of the tech file: uses CIFMAXWIDTH
      >
      > checks layers exactly as how they appear in GDS
      >
      > cifmaxwidth uses width value 0
      >
      > 
      ![image](https://github.com/user-attachments/assets/03715096-793d-4a3a-b813-020e0beded33)
      ### Incorrectly implemented difftap.5 rule
      > Open difftap.mag and try to find out and add missing rule
      
      ![image](https://github.com/user-attachments/assets/40b8a334-a52e-4caa-8e78-224fe4f8ce9b)
      > difftap.5 rule : min tap bound = 0.40, but given in difftap.mag layout = 0.21, still no error
      ![image](https://github.com/user-attachments/assets/35638a29-845e-48f2-8f11-a8268d023fd6)
      ![image](https://github.com/user-attachments/assets/f2b47419-2245-477d-8674-aced8406cf9b)
      > Found missing rules and entered them
      >
      ![image](https://github.com/user-attachments/assets/5b8a1596-0448-4996-8c5c-cc423475039a)
      > Loaded tech file and checked drc again; Errors appeared
      >
      ![image](https://github.com/user-attachments/assets/be8caa42-ba6c-4789-819c-3c51c1462c8f)
      >Redraw the diff-sub-diff junction obeying rules
      >
      >checked for error and no drc errors found

      ![image](https://github.com/user-attachments/assets/7cf7b909-0aed-4caa-9435-ed61b39aa82c)



   - <details>
      <summary><strong> Lab challenge to find missing or incorrect rules and fix them</strong></summary>              
      ![image](https://github.com/user-attachments/assets/6ae95417-286e-4892-a3a7-b2088dc7ba8d)

      ![image](https://github.com/user-attachments/assets/946f2624-510a-4b63-ab05-0264d64bfdfa)
      > After the above edits in tech file, the file is loaded and drc is checked again. This time, the error is correctly reported
      >
      ![image](https://github.com/user-attachments/assets/898d0ccd-8dcd-4adb-b41b-c6720bb2def2)

      > Error solved in copied nwell region after inserting metal tap contact
      >
      ![image](https://github.com/user-attachments/assets/15cea57e-ec2e-499f-9089-8ee5b07b4741)

### Pre-layout timing analysis and importance of good clock tree

- <details>
  <summary><strong> Timing modelling using delay tables</strong></summary>
  
   - <details>
      <summary><strong> Lab steps to convert grid info to track info</strong></summary>
        
      > Open custom inverter layout
           
      > open sky130_inv.mag in magic using sky130A.tech file
      >
      ```bash
      magic -T sky130A.tech sky130_inv.mag &
      ```
      
      > Conditions/Guidelines :
      >
      > 1. The input and output ports must lie on the intersection of horizontal and vertical tracks
      > 2. The width of standard cell must be in odd multiples of track pitch
      > 3. Height should be even multiple of vertical diemension of track pitch
      

      ```bash
      vsduser@vsdsquadron:~/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/openlane/sky130_fd_sc_hd$ less tracks.info
      ```
       
      ![image](https://github.com/user-attachments/assets/6246fd32-ad81-47d0-97a4-024277bf9a79)

      > Type the following commands in tkcon
      ```bash
      help grid

      grid 0.46um 0.34um 0.23um 0.17um
      ```
      ![image](https://github.com/user-attachments/assets/e8c8d5ff-ec06-4a0f-b005-11f52fa27068)

      > Check if input and output are on horizontal and vertical tracks' intersection
      >
      ![image](https://github.com/user-attachments/assets/4890cf91-da77-453a-84c7-633e46c52381)

   - <details>
      <summary><strong> Lab steps to convert magic layout to std cell LEF</strong></summary>
 
      
      > Checking if width of standard cell is odd multiple of xpitch
      >
      > x pitch = 0.46 (from tracks.info file)
      > 
      > width of standard cell = 1.380 = 3 * 0.46 ==> odd multiple
      >
      ![image](https://github.com/user-attachments/assets/81dacad7-1d17-4534-9f02-aa3516052176)

      > Height of std cell = 2.720 = 8 * 0.34(vertical track pitch)
      >
      > Condition 3 satisfied
      >
      ![image](https://github.com/user-attachments/assets/658c0c56-9a75-4c7a-9a89-f2cb163620cd)

      > To convert labels to ports
      >
      > Select port --> Edit menu --> text --> Fill in reqd data
      >
      ![image](https://github.com/user-attachments/assets/1dbd84b8-0bfc-4240-a28c-7012e5b9f0af)

      REf: ![https://github.com/nickson-jose/vsdstdcelldesign](https://github.com/nickson-jose/vsdstdcelldesign)

      > Give custom name and save before extracting LEF file
      >
      ```bash
      save sky130_vsdinv.mag
      ```
      
      ![image](https://github.com/user-attachments/assets/6ae3683e-8c83-404d-a257-d283d0f0b050)
      
      > Open the new inverter
      ```bash
      vsduser@vsdsquadron:~/Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign$ magic -T sky130A.tech sky130_stdinv.mag &
      ```
      > Extract lef file and open it
      >
      ![image](https://github.com/user-attachments/assets/d9e1759f-9047-4eeb-9e75-4fb26b018fd9)
     
      ![image](https://github.com/user-attachments/assets/9993dc2a-3f63-4f95-881f-9b9ffd1bd60c)
         
   - <details>
      <summary><strong>Introduction to timing libs and steps to include new cell in synthesis</strong></summary>  
     
      > Next step is to plug in the lef file to picorv32
      >
      > copy lef file and lib file with cell definition to src folder of  picorv32a designs
      >
      
      ```bash
      vsduser@vsdsquadron:~/Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign$ cp sky130_vsdinv.lef ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/

      vsduser@vsdsquadron:~/Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign/libs$ cp sky130_fd_sc_hd__* ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/

      ```
      list files in destination folder and verify file is copied
     
      ```bash
      vsduser@vsdsquadron:~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src$ ls -ltr
      ```
     
      ![image](https://github.com/user-attachments/assets/fe91577a-06e4-4448-a78f-f1a0bce95c5b)

      >
      >Modify config.tcl
      >
      ![image](https://github.com/user-attachments/assets/9961f2d8-617b-4e68-96ee-06417b480560)
        
   - <details>
      <summary><strong> Lab steps to configure synthesis settings to fix slack and include vsdinv</strong></summary>
     
      > run docker
      >
      
      ```bash
      vsduser@vsdsquadron:~/Desktop/work/tools/openlane_working_dir/openlane$ docker

      bash-4.2$ ./flow.tcl -interactive

      % package require openlane 0.9

      % prep -design picorv32a 

      ```

      ![image](https://github.com/user-attachments/assets/dd38806e-efff-4863-b43c-01b8be9baa5a)

      >  Include newly added lef to openlane
      >
      ```bash
      set lefs [glob $::env(DESIGN_DIR)/src/*.lef]

      add_lefs -src $lefs
      ```
      ![image](https://github.com/user-attachments/assets/7d959ed9-ea0b-47f3-8ecb-cd6db225ca42)

      > Run synthesis
      >
      ```bash
      run_synthesis
      ```
      ![image](https://github.com/user-attachments/assets/68aede23-f26a-4162-aad5-197421ce04eb)

      ![image](https://github.com/user-attachments/assets/6a30f204-864a-4d35-9ea2-a641cb7692aa)

      > Need to see if by inserting inverter with modified parameter can change the results(area and time)
      

      ```bash

      % prep -design picorv32a -tag 04-09_07-24 -overwrite
            
      set lefs [glob $::env(DESIGN_DIR)/src/*.lef]

      add_lefs -src $lefs

      set ::env(SYNTH_STRATEGY) "DELAY 3"

      set ::env(SYNTH_SIZING) 1

      run_synthesis
      ```
      ![image](https://github.com/user-attachments/assets/a7059509-0c80-4bbf-8d95-20d266857917)
      
      ![image](https://github.com/user-attachments/assets/0ce38384-5eaf-4822-8007-3824e91276e9)
      ![image](https://github.com/user-attachments/assets/1cd50bce-4256-4c70-96ff-8ff7c3c6edb7)

      > tns and wns became 0 after seting env(SYNTH_STRATEGY) as DELAY3 and env(SYNTH_SIZING) as 1 after overwriting prep design
      >
      > But area has increase from 147712 to 181730( Strategy selected earlier was AREA, now selected DELAY)
      > screenshot of merged.lef with sky130_vsdinv as MACRO
      ![image](https://github.com/user-attachments/assets/3747fd7f-77a2-4307-9536-606dd11afbac)

      > #### Now we can run floorplan
      >
      
      ```bash
      run_floorplan
      ```
      ![image](https://github.com/user-attachments/assets/19b1bf4d-7b22-4f69-b79e-4816d3121393)
 
      > floorplanning unsuccessfull
      ![image](https://github.com/user-attachments/assets/a6b4dd69-035a-4f8f-9a9e-05f3a7a79726)

      > #### Following floorplan steps one by one
      >
      > 
      ```bash
      init_floorplan
      
      place_io
      
      tap_decap_or
      ```      
      ![image](https://github.com/user-attachments/assets/c7b59b10-4211-4c8c-a328-3d1ed311639c)
     
      ![image](https://github.com/user-attachments/assets/8ea2b261-6238-47a0-beea-7764e8230f24)

      ![image](https://github.com/user-attachments/assets/d03ae619-7d87-453e-a960-c4bca2bc35cb)


      > #### Next is placement
      
      ```bash
      run_placement
      ```

      ![image](https://github.com/user-attachments/assets/3aefc5b2-e45a-4a3e-9a63-72772ed19b81)

      >  Change directory to path containing generated placement def
      ```bash

      cd designs/picorv32a/runs/04-09_07-24/results/placement/
      ```
      
      > Command to load the placement def in magic tool
      
      ```bash
      magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
      ```
      ![image](https://github.com/user-attachments/assets/85de2cd6-3aff-4e66-b8ba-cbecc118582d)

     > Zoomed in to see the custom inv standar cells
     
     [image](https://github.com/user-attachments/assets/f6ea1fb0-927b-4427-a535-adca7cdb6ea4)

     > Use expand command in tkcon window to see internal connections
     >
     ```bash
     expand
     ```

     ![image](https://github.com/user-attachments/assets/dcad27b0-54eb-42fa-a56e-7607a05cc749)
     

- <details>
  <summary><strong> Timing analysis with ideal clocks using openSTA</strong></summary>
  
   - <details>
      <summary><strong> Lab steps to configure OpenSTA for post-synth timing analysis</strong></summary>

      > Doing run_synthesis again for earlier values of synth startegy and all
      >
      > From openlane directory
      >
      
      ```bash

      docker
    
      ./flow.tcl -interactive

      package require openlane 0.9

      prep -design picorv32a

      set lefs [glob $::env(DESIGN_DIR)/src/*.lef]

      add_lefs -src $lefs

      set ::env(SYNTH_SIZING) 1

      run_synthesis
      ```
      ![image](https://github.com/user-attachments/assets/5d822044-557a-43cd-8aa3-1f7896b95b05)


      > ### Create my_base.sdc and pre_sta.conf

      ### my_base.sdc in src folder
      ```tcl
      create_clock [get_ports $::env(CLOCK_PORT)]  -name $::env(CLOCK_PORT)  -period $::env(CLOCK_PERIOD)
      set input_delay_value [expr $::env(CLOCK_PERIOD) * $::env(IO_PCT)]
      set output_delay_value [expr $::env(CLOCK_PERIOD) * $::env(IO_PCT)]
      puts "\[INFO\]: Setting output delay to: $output_delay_value"
      puts "\[INFO\]: Setting input delay to: $input_delay_value"
      
      set_max_fanout $::env(SYNTH_MAX_FANOUT) [current_design]
      
      set clk_indx [lsearch [all_inputs] [get_port $::env(CLOCK_PORT)]]
      #set rst_indx [lsearch [all_inputs] [get_port resetn]]
      set all_inputs_wo_clk [lreplace [all_inputs] $clk_indx $clk_indx]
      #set all_inputs_wo_clk_rst [lreplace $all_inputs_wo_clk $rst_indx $rst_indx]
      set all_inputs_wo_clk_rst $all_inputs_wo_clk
      
      
      # correct resetn
      set_input_delay $input_delay_value  -clock [get_clocks $::env(CLOCK_PORT)] $all_inputs_wo_clk_rst
      #set_input_delay 0.0 -clock [get_clocks $::env(CLOCK_PORT)] {resetn}
      set_output_delay $output_delay_value  -clock [get_clocks $::env(CLOCK_PORT)] [all_outputs]
      
      # TODO set this as parameter
      set_driving_cell -lib_cell $::env(SYNTH_DRIVING_CELL) -pin $::env(SYNTH_DRIVING_CELL_PIN) [all_inputs]
      set cap_load [expr $::env(SYNTH_CAP_LOAD) / 1000.0]
      puts "\[INFO\]: Setting load to: $cap_load"
      set_load  $cap_load [all_outputs]

      ```

      #### pre_sta.conf in openlane folder
      ```tcl
      set_cmd_units -time ns -capacitance pF -current mA -voltage V -resistance kOhm -distance um

      read_liberty -max /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib
      
      read_liberty -min /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib
      
      read_verilog /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/05-09_19-26/results/synthesis/picorv32a.synthesis.v
      
      link_design picorv32a
      
      read_sdc /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/my_base.sdc
      
      report_checks -path_delay min_max -fields {slew trans net cap input_pin}
      report_tns
      report_wns
      ```
  
      ![image](https://github.com/user-attachments/assets/db3c5463-1cb9-4fe9-b3f8-6f9c8a10114b)

      ![image](https://github.com/user-attachments/assets/599f962f-8c85-41f3-a3a4-d149bcb3ef51)


      > #### Run sta in openlane folder
      >
      ```bash
      sta pre_sta.conf
      ```

      ![image](https://github.com/user-attachments/assets/a88888a5-965b-479c-977a-2148f15b8eb7)

      ![image](https://github.com/user-attachments/assets/748cd443-35b9-4868-b154-ec5d2863629d)

      ![image](https://github.com/user-attachments/assets/ab42e187-db96-4b25-938f-143c73c0e640)

      > Need to choose an optimum value of fanout to reduce the slack

    - <details>
      <summary><strong> Lab steps to optimize synthesis to reduce setup violations</strong></summary>     

      ```bash
      prep -design picorv32a -tag 25-03_18-52 -overwrite

      set lefs [glob $::env(DESIGN_DIR)/src/*.lef]

      add_lefs -src $lefs

      set ::env(SYNTH_SIZING) 1

      set ::env(SYNTH_MAX_FANOUT) 4

      echo $::env(SYNTH_DRIVING_CELL)

      run_synthesis

      ```

      ![image](https://github.com/user-attachments/assets/4506dca7-1f62-4bc0-a3e7-820708aabe6a)


      > synth result not good
      >
      
      ![image](https://github.com/user-attachments/assets/8b24b356-7876-4d5b-b270-af7aca85d8b2)

      > Run sta
      
      ![image](https://github.com/user-attachments/assets/3253b4a3-6575-445d-87bd-50615c2d2069)
                                                                                     
   - <details>
      <summary><strong> Lab steps to do basic timing ECO</strong></summary>

      ![image](https://github.com/user-attachments/assets/c64d892d-c0ce-4f7c-a355-95b046bed0d0)

      > OR_2_2 driving 4 (drive strength 2, fanout 4)
 
      ```bash

      report_net -connections _11672_

      help replace_cell
      
      replace_cell _14510_ sky130_fd_sc_hd__or3_4

      report_checks -fields {net cap slew input_pins} -digits 4
      ```
 
      
      ![image](https://github.com/user-attachments/assets/540735fa-f3d0-4940-8660-79a3dba7dc4d)
     
      ![image](https://github.com/user-attachments/assets/c62b5962-60f7-4bcd-aab9-f4278d7f5d67)


      ```tcl
                    0.07    0.00    3.51 v _14510_/C (sky130_fd_sc_hd__or3_2)
                    0.21    1.04    4.55 v _14510_/X (sky130_fd_sc_hd__or3_2)
       4    0.01                           _11672_ (net)

      ```

      became

      ```tcl
                    0.0792    0.0000    3.5212 v _14510_/C (sky130_fd_sc_hd__or3_4)
                    0.1349    0.6755    4.1967 v _14510_/X (sky130_fd_sc_hd__or3_4)
      4    0.0089                                _11672_ (net)

      ```
      > Similarily by reducing slack by replacing overloaded drivers, we can cut significant slack
 
      ```bash
      % replace_cell _14510_ sky130_fd_sc_hd__or3_4

      % replace_cell _14514_ sky130_fd_sc_hd__or3_4

      % replace_cell _14481_ sky130_fd_sc_hd__or4_4

      % replace_cell _14506_ sky130_fd_sc_hd__or4_4

      ```    

      ![image](https://github.com/user-attachments/assets/59d189ae-afb6-4aab-93a2-9156d9a96901)

      ```bash
      report_checks -fields {net cap slew input_pins} -digits 4
      ```
      > Started from -23.9, reduced upto -22.6
      >
      
      ![image](https://github.com/user-attachments/assets/a2b7442b-3734-4a0f-8658-a9d07fbb85c3)

      > Replaced instance
      >
      ![image](https://github.com/user-attachments/assets/02e37941-82dc-45e0-8002-53f58072977f)

- <details>
  <summary><strong> Clock tree synthesis TritonCTS and signal integrity</strong></summary>

   - <details>
      <summary><strong> Lab steps to run CTS using TritonCTS</strong></summary>  

      > make a copy of the old old netlist
      >
     
      ![image](https://github.com/user-attachments/assets/1727d95b-cb53-4fae-aece-d4f9111c75d5)

      > Insert this updated netlist to PnR flow. we can use `write_verilog` and overwrite the synthesis netlist , finally exit
      
      ![image](https://github.com/user-attachments/assets/88f196cc-0253-489f-9d19-9a7a6f3b38ea)

      
      > RUn upto placement and cts on the synthesis where wns and tns = 0
      >
      ```tcl
      
      prep -design picorv32a -tag 04-09_19-26 -overwrite
      
      
      set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
      add_lefs -src $lefs
      
      
      set ::env(SYNTH_STRATEGY) "DELAY 3"
      
      
      set ::env(SYNTH_SIZING) 1
      
      
      run_synthesis
      
      
      init_floorplan
      
      place_io
      
      tap_decap_or
      
      
      run_placement
      
      # Incase getting error
      unset ::env(LIB_CTS)
      
      # With placement done we are now ready to run CTS
      run_cts
      ```

      ![image](https://github.com/user-attachments/assets/e2e58355-de4f-4974-afc1-f52ecbe2c17f)

      ![image](https://github.com/user-attachments/assets/aa5f3600-7e2b-4a45-a50c-747dbc63d45c)

      ![image](https://github.com/user-attachments/assets/f91b2ea5-16a2-4a54-9fb5-dd20c328e4cd)

      ![image](https://github.com/user-attachments/assets/5c6ad4d2-9666-49f1-ba7c-5931d0be4d09)


      ![image](https://github.com/user-attachments/assets/707069ec-ffb3-4dd6-a76d-6a0754b62da9)

      ![image](https://github.com/user-attachments/assets/79ebc2d8-9651-49ee-a907-47197fa6779c)

      ![image](https://github.com/user-attachments/assets/8a58b2d2-5c3e-4d7e-b07f-13ef8f96c8b3)

      ![image](https://github.com/user-attachments/assets/d7964c22-af8c-4563-9cab-2b731387878e)

      > CTS Successful
