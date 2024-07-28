Instruction length:
- Fixed length:
    -  (+) easier to decode single instruction in hardware
    -  (+) easiser to decode multiple instrucitons concurrently
    -  (-) wasted bits in instructions, need to transfer more bits from memory to processor
    -  (-) harder to extend ISA (how to add new instructions?)
- Variable length:
    - length of instrucitons different

- Superscalar processor: fetches and executes multiple instructions at the same time
![alt text](image-6.png) 
- you can have no dependency among the decoders for each instruction
- each decoder knows where exactly where the instruction is
- but if you have variable length instructions
- decoders must be much more complex


![alt text](image-7.png)
- you construct a tree based on frequency
- You only need necessary amount of bits to encode each instruction
- more logic to decoe each instruction

- When you transfer bits from memory you dont just transfer 32 bits, youre transferring a big chunk of data as well, in variable length instruction setting you can encode more instructions in those 128 bytes.

- Unifrom decode: same bits in each instruction correspond to the same meaning
    - so essentially bits at certain locations are always reserved for a specific purpose if its fixed length
    - Opcode is always in the same location
    - Ditto operand specifiers, immediate values
    - (+) makes it easier to decode, simpler hardware
    - (+) enables parallelism: generate target address before knowing the instruction is a branch
    - (-) restricts instructrion format or wastes space
    - in variable length, hard o think of uniform decode as a property of instructions of different length, it can be a property of instructions of the same length
    - uniform decode usually goes with fixed length
- Non-uniform decode:
    - unpredictable, opcode can be the 1st-7th byte in x86

MIPS instruction format:
- allows for very simple decoding
- 4 bytes per instruction , regardless of format
- format and fields easy to extract in hardware



RISC:
- simple instructions
- fixed length
- unifrom decide 
- few addressing modes

Number of registers:
![alt text](image-8.png)
- affects:
    - number of bits used for encoding register address
    - number of values kept in fast storage (register file)
    - (uarch) size,  access time, power consumption of register file
- large number of registers:
    - enables better register allocation (and optimizations) by compiler -> fewer saves/restores
    - larger instruction size
    - larger register file size


Addressing modes:
- Register indirect: m[r2], r2 = ptr
- Memory indirect: m[m[r2]], r2 stores a pointer then the pointer points to value
- Addressing mode specifies how to obtain an operand of an instruction 
    - register
    - immediate 
    - memory (displacement, register indirect, indexed, absolute, memory indirect, autoincrement, autodecrement)
- more modes: