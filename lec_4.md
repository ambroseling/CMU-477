Instruction length:
- Fixed length:
    -  (+) easier to decode single instruction in hardware
    -  (+) easiser to decode multiple instrucitons concurrently
    -  (-) wasted bits in instructions
    -  (-) harder to extend ISA (how to add new instructions?)
- Variable length:
    - length of instrucitons different

- Superscalar processor: fetches and executes multiple instructions at the same time
![alt text](image-6.png) 
- you can have no dependency among the decoders for each instruction
- each decoder knows where exactly where the instruction is
- but if you have variable length instructions
- decoders must be much more complex