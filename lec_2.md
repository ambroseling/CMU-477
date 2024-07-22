Computer architecture today
- heterogenous processors and accelerators 
- hybrid main memory
- persistent memory storage
- general purpose GPUs
- persistent memory/storage
- every component and its interfaces as well as entire system designs are being re-examined


Fundamental Concepts
- 3 key concepts
    - computation
    - communication
    - storage
- Computing system:
    - computing unit -> communication -> storage unit / memory system
    - processing: control (sequencing)
    - datapath
    - memory: (program and data)

- Von Neumann Model/Architecture
    - its also called stored program computer
    - 2 key properties:
        - stored program
            - instructions stored in a linear memory array
            - memory is unified between instructions and data
            - the interpretation of a stored value depends on the control signals
            - so when is a value interpreted as an instruction?
        - sequential instruction processing:
            - one instruction processed (fetched,executed,processed)
The Dataflow model (of a computer)
- in the von neumann model: an instruction is fetched and executed in control flow order
    - as specified by the instruction pointer
    - sequential unless explicit control flow instruction
- Dataflow model: an instruction is fetched and executed in dataflow order
    - when its operands are ready
    - there is no instruction pointer
    - instruction ordering specified by data flow dependence
        - each instruction specifies who would receive the result
        - an instruction can fire whenever all operands are received
    - potentially many instructions can execute at the same time

Sequential:
```c
v = a + b;
w = b * 2;
x = v - w;
y = v + w;
z = x * y 
```

Dataflow:
- in a dataflow machine, a program consists of data flow nodes
- a data flow node fires (fetched and executed) when all its inputs are ready
- data flow node and its ISA representation:
    - | * | R | ARG 1 | R | ARG 2 | Dest of Result |
- data flow nodes
    - codnitional: bool 
    - relational: gt or lt
    - barrier synch: if the nodes are ready, wait for all threads to finish executing  (bulk synchronous processing)  / there are also with load imbalance (a bunch of threads have to wait for 1 thread to finish)

ISA level tradeoff: instruction pointer
- do we need an instruction pointer in the ISA?
    - YES: control driven, seqeuntial execution
    - an instruction is executed when the IP points to it
    - IP automatically changes sequentially (except for control flow instructions)
    - NO: data driven, parallel execution
        - an instruction is executed when all its operand values are availble (data flow)
        - 
ISA vs Microarchitecture Level Tradeoff
- a similar tradeoff (control vs data-driven execution) can be made at the microarchitecture level
- ISA: specifies how the programmer sees instructions to be executed
    - programmer sees a sequential, control-flow execution order 
    - programmer sees a data-flow execution flow
- micro architecture: how the underlying implementatino actually executes instructions
    - microarchitecture can execute instructions in any order as long as it obeys the semantics specified by the ISA when making the instruction results visible to software
    - programmer should see the order specified by the ISA



ISA:
- instructions:
    - opcodes, addressing modes
    - data types
    - instruciton types and formats
    - registers, condition codes
- Memory:
    - addres space, addressability, alignment
    - virutal memory management
- Call, interrupt/Exception Handling
- Access Control, Priority/Priviledge
- I/O: memory mapped vs Instructions
- Task/thread management
- power and thermal management
- multi threaded support, multi processor support
