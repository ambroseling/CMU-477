Intro to microarchitecture
- Branch delay spot: introduced to combat certain issues
    - a load may take multiple cycles
    - so you may try to run instructions independent of the load to not make hardware stall
    - but that means introducing the microacrchitecture, not a good thing
- How would you design a new ISA?
    - design choices, semantic gap?
    - design point?

- RISC:
    - reduced instructions
    - fixed length
    - uniform decode

- CISC:
    - complex reduced instructions
    - many addressing modes
    - variable length
    - non uniform decode

Microarchitecture basics:
- processing an instruction
- 1. AS = atchtiectureal state before an instruction is processed 
- 2. process instruction 
- 3. AS' architectural state afer an instruction is processed


Processing an instruction: transforming AS to AS' according to the ISA specification of the instructino

ISA specifies abstractly what AS' should be, given an instruction and AS:
- it defines an abstract FSM where
    - State: programmer visible state
    - Next-state logic: instruciton execution specification

- microarchitecture implements how AS is transformed to AS'
    - there are many choices in implementation
    - we can have programmer invisible state to **optimzie** the speed of instruction execution: mutliple state transitions per instruction
        - choice 1: AS -> AS' (transform A to A' in a single clock cycle)
        - choice 2: AS  ->  MS1 -> AS + MS2 -> AS + MS3 -> AS'

- very basic instruction processing engine:
    - each instruction takes a single clock cycle to execute
    - only combinational logic is used to implement instruction execution
    - no intermediate, programmer invisible state updates

**NOTE**: programmer visible state (instructions transform the values of programmer visible state)
    - memory
    - registers
    - program counter

- Single Cycle machines
    - each instruction takes a single clock cycle
    - all state updates made at the end of an instruction's execution
    - slowest instruction dictates your clock cycle time
    - control signals are generated in the same clock cycle as the one during which data signals are operated on 
    - everything related to an instruction happens in one clock cycle (serialized processing)

- Multi-cycle machines
    - instruction processing broken into multiple cycles/stages
    - state updates can be made during an instruction's execution
    - architectural state updates made only at the end of an instructions execution
    - advance only single cycle: the slowest stage determines cycle time
    - control signals needed in the next cycle can be generated in the current cycle
    - latency of control processing can be overlapped with latency of datapath operation (more parallelism)
    - such that when you are processing the next cycle, the control signals are ready, then you can make your clock cycles shorter
    - try to overla the latency 
- Both single cycle and multi-cycle machine literally follow the von Neumann model at the microarchitecture level

- Instruction processing: cycle
    - instructions are processed under the direction of a control unit
    - instruction cycle: sequence of steps to process an instruction
    - 1. fetch
    - 2. decode
    - 3. evaluate address
    - 4. fetch operands
    - 5. execute
    - 6. store result
    - NOTE: not all instructions require all 6 phases
    - memory can be main bottleneck
- Functional units (execution unit)
    - functional units transform data (AS) to data(AS')
    - Datapath: consists of hardware elements that deal with and transform data signals
        - functional units that operate on data
        - hardware structure that enable the flow of data into the functional units and registers
        - storage units that store data


Performance analysis
- execution time of an instruction: $CPI x {clock cycle time}$
- execution time of a progam: sum over instructions [{CPI} x {clock cycle time}] 
    - {# of instructions} x {Average CPI} x {Clock cycle time}

Single cycle microarchitecture performance
- CPI = 1
- clock cycle time = long


Multi cycle microarchitecture performance
- CPI = different for each instruction
    - average CPI -> hopefully small
- clock cycle time = short


We can assume:
- combinational read
    - output of the read data port is a combinational function of the regsiter file contents and the corresponding read select port
- synchronous write:
    - the selected regsiter is updates on the positive edge clock transition when write enable is asserted
- single cycle, synchronous memory
    - contrast this with memory that tells when the data is ready
    
R-type ALU instructions
-

I-type ALU instructions
- 