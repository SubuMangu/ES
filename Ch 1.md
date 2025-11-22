# Ch 1
## Embedded Systems
- **Def:** An embedded system is a small computing system embedding in a larger electronic device, performing a single(or small number of) tasks.
- Eg, If phone is a large electronic device, then camera is an embedded system and its task is to capture photos and video.

## Features of Embedded System
1. **Single functioned system:** performs a single or small set of specific tasks.
2. **Interaction with physical environment:** Sensors collect data from physical environment, then it processes and then actions are performed by the actuators.
3. **User Interface:** Gives a feel of controlling the device processes behind the scenes are kept hidden.
4. **Dependable systems**, since they are reliable. So they are used in many safety critical systems.
5. **Tightly constrained system:** It must satisfy few constraints like size ,performance, budget or power consumption.
6. **Real time systems**
7. **Hybrid system:** include both analog and digital components
8. **Reactive systems:** Continuoues interaction with environment behaviour of the system is dependent on the events occurring in the environment.
## Design Metrics
1. **System Cost:** It is the cost required in building and maintaining the embedded system.It is of 2 types: **Recurring** and **Non Recurring Cost**

|Non Recurring Cost|Recurring Cost|
|-|-|
|One time expenditure incurred in design of the system.| Recurring cost are regular expenses associated with the regular operation, maintenance and production of a product or service.|
|Eg, Generating masks of VLSI chips requires very high cost. Once mask is designed, it can be replicated to produce large number of similar chips|Eg, Software subscriptions, maintenance contracts, energy cost, etc.|

2. **Size:** It is measured in silicon area for hardware components. For software component size is measured in code size. 
3. **Performance:** Refers to the speed of the design system.ASIC has higher performance than FPGA.
4. **Power Requirements:** Embedded systems are expected to have light weight and long battery life. 
5. **Design flexibility:** It refers to the effort needed to modify a system.FPGA is more flexible than ASIC.
6. **Design Turnaround Time:** This is  the  time  needed to  complete  the  design  starting  from specification upto taking it to the market. 
7. **System  maintainability:**  This  refers  to  the  ease  of  maintaining  and monitoring  the health of the system after it has been put into the field.
8. **Testing and verification of functionality:** It  refers  to  the  ability  to  check  the  system functionality and get confidence regarding the correct operation of it. 
### ASIC vs FPGA
|ASIC(Application Specific Integrated Circuit)| FPGA(Field Programmable Gate Array)|
|-|-|
| Custom made chip design for specific application| Reconfigurable chips made up of programmable logic blocks|
| Very fast and efficient, but cannot be reprogrammed| Can be reprogrammed to perform different hardware task, but slower than ASIC|
|Eg, Chip inside smartphone camera|Eg, Cisco uses FPGA to design routers during its prototyping stage|

## ES Design flow
- These are the phases that we have to pass while designing embedded system:
1. **System Specification:** 
- Less detailed and surface level specification of the functionality of a embedded system using UMLs,Pseudocode,Activity Diagrams,etc.
- Uses **Model Simulators/Checkers** to verify.
- It requires **system library**, which is a collection of readymade hardware and software and **system synthesis tool** for further step.
2. **Behavioural specification:**
- Detailed representation of state of a system at each step using UMLs,Pseudocode,Activity Diagrams,etc.
- Software processes are run on general purpose registers.
- Hardware processes are run on dedicated hardware.
- Verified by **hardware software co-simulators**.
3. **RT Specifications**
- Describes design at register level.
- Software processes are converted to machine code by compilers used for final implementation.
- Hardware processes are broken down into a netlist of library components using **behavioural library cores**and **behavioural synthesis tools**.
- Hardware RT specifications are verified by **RTL simulators**.

4. **Logic Specification**
- Represents hardware design as boolean equations using **RT library/components**.
- Verified by **Gate level simulators**.
- Broken down into simple logic get representation by **logic library** and **logic synthesis tools** for final execution.

<p align="center"><img src="Images/Screenshot 2025-11-22 165951.png" width="" height=""></p>


