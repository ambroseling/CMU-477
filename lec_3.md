ISA architectures and its tradeoffs
- ISA:
    - agreed upon interface btw software and hardware
    - mciroatchitecture: specific implementation of an ISA
    - micorproessor: ISA, uarch, circuits 
- Microarchitecture: 
    - it is a **specific** implementation of the ISA under specific design constraints and goals
    - these design points can be (determines how you implement the ISA):
        - cost, performance, max power consumption, energy consumption, availability, time to market
    - things done in hardwdare without exposure to software:
        - pipelining
        - in-order VS out-of-order instruction execution
        - memory access scheduling policy
        - speculative execution
        - superscalar processing
        - clock gating
        - caching? Levels, size, associativity, replacement policy
        - prefetching?
        - votlage/frequency scaling

- ISA-level tradeoffs
- Microarchitecture level tradeoffs
- systme and task level tradeoffs

- Many different ISAs over decades
    - x76, PDP-x, VAX, CDC6600, RISC ISAs: Alpha, MIPS, SPARC,ARM

- Fundamental differences:
    - how instructions are specified and what they do
    - how complex are the instructions

- Intructions:
    - opcode: what the isntruction does
    - operands: who it is to do it to
