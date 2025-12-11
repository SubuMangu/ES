# Ch 2
## ARM7 Structure
<p align="center"><img src="Images/Screenshot 2025-12-09 123854.png" width="" height=""></p>

**Buses Used**
- `A[31:0]` : Address bus
- `DATA[31:0]` : Input data
- `DOUT[31:0]`:Output data

1. **Instruction Pipeline and Read data Register:**
- Instruction pipeline executes series of instructions on DATA.
-  Read data register gets the content of memory location pointed to by the address bus lines A[31:0].The external 32-bit data-in lines DATA[31:0] put the content into this register.
2. **Instruction Decoder and Control Logic:**
- Decodes instruction in instruction pipeline.
- Outputs a number of control signals for interactions with peripherals.
3. **Address Register:**
- Holds the the address of the next instruction or data to be fetched.
-  The  input  signal  ALE  determines  the  time upto which the register’s content will remain available on the A[31  :  0]  lines.  Content is  available  as  long  as  ALE  remains  low.
4. **Address Incremeter:**
-  It  increments the Address Register’s value by an appropriate amount to point to the next instruction/data.
5. **Register Bank:**
- It contains 31 number of 32 bit registers, out of which six of them are status registers.
6. **Booth's multiplier:** Used in multiplication instruction.
7. **Barrel shifter:** used in shifting operation
8. **ALU:** A 32 bit ALU performs arithmetic and logical operations.
9. **Write data register:** Writes the output in DOUT.

## Control and Status Signals in ARM7
### 1. Processor mode signals
1. `nM[4:0]` 
- Identifies mode of the processor.
- Output bits are inverses of the mode of the processor.
- There are six modes in an ARM7 processor: User, Fast Interrupt (FIQ), Interrupt (IRQ), Supervisor, Abort, Undefined.

### 2. Clock Signals
1. `MCLK`**(Master Clock)**
-  In  phase 1 MCLK is low and in phase 2 it  is high.
2. `nWAIT`
- Connected with MCLK by AND operator
- Used for waiting integer number of MCLK cycles, for slower peripheral devices by holding nWAIT low.
- nWAIT can only change when MCLK is low

### 3. Memory interface signals
1. `A[31:0]`:Address bus lines
2. `DATA[31:0]`: Input data bus
3. `DOUT[31:0]`: Output data bus
4. `nENOUT`**(Enable Output)**
- A status signal activated(made low) when DOUT has some valid data to be written into memory
- Can bu utilized to create a bidirectional bus with DATA for the memory.
5. `nMREQ`**(Memory Request)**
-  Indicates(when low) that the processor requires memory access
6. `SEQ`
- Indicates(when high)  indicates that the address used in  the  following  cycle  is  either  the  same  as  the  last  memory  address,  or  is  4  greater  (i.e.,  the 
next  word  address).
- Basically checks if memory blocks are in sequence

**Note:** Both nMREQ and SEQ is used for bulk memory fetching. The  two  signals  nMREQ  and  SEQ  together  indicate burst  activity  one  cycle  advance.

7. `nRW`: For  a  read  cycle,  the  signal  is  low,  for  a  write  cycle,  it  is  high.
8. `nBW` : It  is  high  for  a  word transfer  and  low  for  a  byte  transfer.
9. `LOCK`: When LOCK  is  high,  the  memory  controller  should  not  allow  any  other  device  to  access  memory  till LOCK  becomes  low. Used in swap instruction.

### 4. Memory management signals
1. `nTRANS`
- nTRANS  =  0  indicates  that  processor is  in  user  mode  and  address  translation  should  be  turned  on.  
- nTRANS=1, physical address has been translated to virtual address
2. `ABORT`
- ABORT  is  an  input  to  the  processor.  This  allows the  memory  system  to  tell  the  processor  that  the  requested  access  is  not  allowed.

### 5. Configuration Signals
1. `PROG32`**(Fetching Instruction)**
- When high it fetches instruction from 32 bit address space.
- When low fetches instruction from 26- bit address space.
2. `DATA32`**(Fetching data)**
- Fetches data from 32-bit address space
- Fetches data from 26-bit address space
3. `BIGEND`
- When equals to 1, big endian else, little endian
### 6. Interrupts
1. `nIRQ`: Normal interrupt
2. `nFIQ`: Forceful interrupt
### 7. Bus Control Signals
1. `ALE`**(Address Latch Enable)**:
- Address latch mean to store/hold the address so that operations can be performed on addresses one by one
- If low(latch disabled), new address can be fetched from address bus A[31:0]. ALE can be low only when MCLK=0.
- If high(latch enabled), the current address is on only.
2. `DBE`**(Data Bus Enable)**
- DBE is a control signal used to enable or disable access to the external data bus DOUT[31:0] and DATA[31:0].
- If DBE = 1, ARM7 drives / receives data on external bus.
- If DBE = 0,ARM7 disconnects (tri-states) external data pins, allowing DMA or another master to drive bus.
### 8. Special Signals
1. `nEXEC`
- `nEXEC` is high when the instruction in the execution unit is not being executed, for example, it has failed its condition code check.
2. `nRESET`
- `nRESET` is low-level sensitive reset signal for the processor.

## ARM Pipeline
### 1. 3-stage pipeline
-  fetch–decode–execute pipeline.
- **Fetch:** Read instructions from memory and Increment address value.
- **Decode:** Convert instructions to control signals.
- **Execute:** Execute control signals.

**Drawbacks**
- Pipeline stall (cannot fetch the next instruction ,if current instruction use the same memory at same time)

### 2. 5-stage pipeline
- Solves pipeline stall.
- Instruction and data memory has been separated so that next instruction can be fetched, while other one uses the data.
- The Register read step is moved to decode stage, unlike it was there in fetch stage in three stage pipeline.
- Execution stage is splitted into three stages: arithmetic operation ,memory access right result.

### 3. 6-stage pipeline
- 64 bit instruction and data buses are introduced, which can simultaneously take two 32 bit instructions.
- Decode stages divided into two stages:
    1. **Decode**
    2. **Register:** Reads the register to be used.
### 4. 8-stage pipeline
- Both instruction and data access are distributed across two pipeline stages.
- Shift operation has been separated into a separate pipeline stage.
- Execution unit split into three different pipelines can operate concurrently and commit instructions out of order also.

## Instruction Set Architecture(ISA)
### 1. Registers
- It has 16 general purpose registers (GPRs) from R0 to R15, CPSR and SPSR.
- R15 is program counter.
- R14 is link register, used to store the return value from subroutine.
- R13 is stack pointer, points the top of the function call stack or any stack.

#### CPSR(Current Program Status Register)
- It holds the current state of the processor.
- Accessible in all modes.
- It has the following components:
    1. **Condition Flags(NZCV)**
        - **N(Negative):** Set when result of operation is negative (sign bit = 1).
        - **Z(Zero):** Set when result of operation is zero.
        - **C(Carry):** Carry(during addition) and non occurance of borrow(in subtraction).
        - **V(Overflow):** Set when signed (two’s complement) overflow occurs. Especially in case of addtion of 2 negative values.
    2. **I(Normal Interrupt)**
    3. **F(Fast interrupt)**
    4. **T(Thumb mode):** If high, THUMB instruction is used, else ARM instruction is used.

    <p align="center"><img src="Images/Screenshot 2025-12-10 122838.png" width="" height=""></p>

#### SPSR(Saved Program Status Register)
- Stores a copy value of CPSR before exception raise, last execution state can be restored.
- Accessible in all modes except user modes.

#### Execution modes
1. **User  mode**  is  used  to  run  the  application  code.  Once  in  user  mode,  the  CPSR  cannot be  written  to.  Mode  can  only  be  changed  when  an  exception  is  generated.
2. **Fast interrupt processing mode (FIQ)** supports high speed interrupt handling. Generally it  is  used  for  a  single  critical  interrupt  source  in  a  system.
3. **Normal  interrupt  processing  mode  (IRQ)**  supports  all  other  interrupt  sources  in  a 
system.
4. **Supervisor mode (SVC)** is entered when the processor encounters a software interrupt 
instruction. These are standard ways to invoke operating system services. Upon reset, 
ARM  enters  into  this  mode.
5. **Undefined instruction mode (UNDEF)**  is  entered  if  the  fetched  opcode  is  not  an  ARM instruction  or  a  coprocessor  instruction.
6. **Abort  mode**  is  entered  in  response  to  memory  fault,  for  example,  an  instruction  or data  fetched  from  an  invalid  memory  region.

- R0-R7  and CPSR are common to all modes
- Each  of  the  other  modes  have  their  own  R13  and  R14  registers  so  that  each  mode  has  its  own stack  pointer  and  link  register. 
- In exception modes(all the modes except user mode), SPSR is used.
#### CPSR vs SPSR
|CPSR|SPSR|
|-|-|
|Holds the current state of the processor| Stores a copy value of CPSR before exception raise.|
| Used while executing instruction| Used in exception handling|
| Accessible in all modes| Accessible only in exception modes|
#### Data Types
- The  ARM  instruction  set  supports  six  different  data  types,  namely,  8-bit  signed  and  unsigned, 16-bit  signed  and  unsigned,  32-bit  signed  and  unsigned.
- The  ARM  processor  instruction  set 
has  been  designed  to  support  these  data  types  in  little-  or  big-endian  format.  

### 2. Data Processing Instructions
- It supports the following types of operation:
    1. **Arithmetic:** ADD,SUB,MUL
    2. **Logical:** AND,OR,XOR
- It supports three addressing mode:
`operator var,op1,op2`
- `var`:variable
- `op1`:first operand
- `op2`:second operand
- Instead of variable, we can use numbers directly as **immediate operands**.
- A number must satisfy the following condition to be used as an immediate operand.
$$ n=\text{i ROR}\left(2*r\right) $$
$$ 0\le i \le 255\text{ and }0 \le r \le 15 $$
$$ROR:\text{Right shift operator}$$
Ex: ff000000 can be used as immediate operand since it satisfies the above rule with i=255(ff in hexadecimal) and r=0
- But 511(fff) can't be used as an immediate operand since it doesn't satisfy the above rule.

### 3.Data Transfer Instructions
- There are three types of data transfer instructions:
1. **Single Register Transfer:**
- It involves single register transfer instruction between two registers or few registors.
- It supports 1,2 and 4 bytes of data transfer.
**Examples:** 
- LDR R0,[R8] :load  content  of  memory  location  pointed  to  by  R8  into  R0.
- LDR R0, [R1, –R2]: load  content  of  memory  location  pointed  to  by  R1 – R2  into  R0. 
- LDR R0, [R1, +4] load  content  of  memory  location  pointed  to  by  R1 + 4  into  R0.
2. **Multiple register transfer:**
- It involves multiple registered transfer instructions between two or few registers.
- It uses **base plus offset addressing** or **auto index addressing** for one by one execution of multiple instructions.
- It is of two types:
    1. **Post indexed**: First data is loaded or stored then address is incremented to next.\
    Eg, LDR R0,[R1],+16 :Loads  R0  from  memory  location  pointed  to  by  R1,  then  adds   16  to  R1.
    2. **Pre indexed**: First the address is incremented then load or store operation.\
    Eg, LDR R0, [R1, +4]! : load  content  of  memory  location  pointed  to  by  R1  +  4  into  R0, R1  is  also  incremented  by  4.

<p align="center"><img src="Images/Screenshot 2025-12-10 160645.png" width="" height=""></p>

3. **Block data transfer**
- It involves load and store of multiple instructions at a time using multiple registers parallely.
- Block data transfer operations used are LDM(Load Multiple) and STM(Store Multiple).
- It can be done in two ways.
    1. **Using Stack:** Here SP represents stack pointer
    <p align="center"><img src="Images/Screenshot 2025-12-10 163905.png" width="230" height=""></p>
    <p align="center"><img src="Images/Screenshot 2025-12-10 164033.png" width="" height=""></p>
    2. **Transfer around memory**
    <p align="center"><img src="Images/Screenshot 2025-12-10 233703.png" width="" height=""></p>
### 4. Multiplication Instruction
- It support two datatypes:
    1. **Integer(32 bit):** MUL,MULA 
    2. **Long Integer(64 bit):** SMULL,SMULAL,UMULL,UMULAL
    <p align="center"><img src="Images/Screenshot 2025-12-10 234525.png" width="" height=""></p>
- Few restrictions regarding source and destination:
    1. Destination and the first operand cannot be in the same register.
    2. PC  (R15)  cannot  be  used  for  multiplication.
### 5. Software Interrupt
- The Software Interrupt(SWI) forces CPU to enter into supervisor mode.
- The format of instruction is:
$$\text{SWI \#n}$$
- Here $n$ is a 24 bit number to represent an operation.
- Eg, `SWI #5`,may be request to file open denied.
- SWI can bu used for system calls.
### 6.Conditional Instruction
- Use 4 flag(NZCV), to represent a condition.

<p align="center"><img src="Images/Screenshot 2025-12-11 095158.png" width="" height=""></p>

<p align="center"><img src="Images/Screenshot 2025-12-11 095246.png" width="" height=""></p>

### 7. Branch Instruction
- The branch instructions follows the certain conditions:
    1. All  branches  are  relative  to  the  program  counter.
    2. Jump  is  always  within  a  limit  of  ±32  MB.
    3. Conditional  branches  are  formed  by  using  the  condition  codes  as  discussed  earlier.
    4. Subroutine  call  instruction  is  also  modelled  as  a  variant  of  branch  instruction.
- The 2 opcodes reserved for branching are B(standard branch) and BL(branch with link,current value of PC+4 is saved in link register R14).

<p align="center"><img src="Images/Screenshot 2025-12-11 100638.png" width="" height=""></p>

- Some instructions are also used for branch exanchage BX and BLX. These are similar to B and BL, but performs exchange of instructions between ARM and THUMB.

### 8.Swap instructions
- Swap is an atomic operation in which a memory read is followed by a memory write which moves byte or word between registers and memory.
- It's format is:

$$\text{SWP }R_d,R_m,[R_n]$$
$$\text{SWPB }R_d,R_m,[R_n]$$

- Now let's understand how does this function work.
1. It  is  a  two-cycle  operation.
2. Content of memory location pointed to by register Rn is copied into a temporary space.
3. Content  of  register  Rm  is  copied  into  the  memory  location.
4. Content  of  the  temporary  space  is  copied  into  the  register  Rd.

<p align="center"><img src="Images/Screenshot 2025-12-11 103150.png" width="" height=""></p>




