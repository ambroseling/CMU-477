Find the difference btw 2 archs:
- Cost
- Design: a good design is designed for what you want

Goals
- understand the principles
- understand the precedents
- based on such understanding:
    - evaluate tradeoffs of different designs and ideas
    - develop principled designs
    
- Computer architect
    - Look backward: understand tradeoffs and designs, upsides/downsides, past workloads
    - Look forward: be the dreamer and create new designs, listen to dreamers, push the state of the art
    - Look up: understand important problems and their nature, develop ideas to solve these problems
    - Look down: understand the capabilities of the underlying technology, predict and adapt to the future of technology

- How does an assembly program end up executing as digital logic?
- What happens in between?
- How is a computer designed using logic gates and wires to satisfy specific goals?
- architect's view: how to design a computer that meets system design goals

- Multi core systes:
    - design a single core for the processor, but its hard to do (will explain later)
    - its easier to design multi core, but it makes life harder for the programmer since they have to parallelize their program to use all the cores

- Problem -> Algorith -> Runtime -> ISA -> Microarchitecture -> Logic -> Circuits -> Electrons

- power of abstraction
    - levels of transformation create abstractions
        - abstraction: a higher level only needs to know about the interface to the lower level, not how the lower level is implemented
    - high level language programmer does not really need to know what the ISA is and how computer executes the instructions

- abstraction improves productivity
    - no need to worry about decisions made in underlying levels
    - programming in Java VS C

- What if the program you wrote is very slow?   
- What if the program consumes too much energy?

- Key goals:
    - understand how a processor works underneath the software layer
    - how decisions in hardware affect the software/programmer
    - enable you to be comfortable in making design and optimization decisions that cross the boundaries of different layers and system components.

There is an example of unexpected slow downs in mutli core systems:
- Core 0: matlab (low pri)
- Core 1: gcc (high pri) -> 3x slow down


they have a shared memory system
- matlab + gcc
    - l2 cache + l2 cache
        - interconnect
            - dram memory controller 
                - dram bank 0
                - dram bank 1
                - dram bank 2
                - ...
matlab sends a lot of memory requests
dram memory controller is unfair, and prioritizing matlab

How does the dram bank operation work?
- Access address: (Row 0, Column 0)
- Goes into row decoder, gets a row, stores in row buffer
- memory controller sends column address 0, muxes it out to get the data at that column for the row already in the buffer
- accessing a different column for the same row is pretty fast, since it is a HIT, already in the row buffer, very quick
- but if you access a different row, there is a CONFLICT, you write the row back to original array address
- a row-conflict memory access takes significantly longer than a row hit access
- current controllers take advantage of the row buffer
- the mmeory controller prioritizes memory accesses that are row hit, memory controller says i have currently row 0 open in this bank, so prioritizes all the access to row 0 first
- commonly used scheduling policy:
    - 1. row hit first: service  
    - 2. oldest first: then service older accesses first
- maximize the throughput and reduce latency
- problem comes:
    - multiple applications share the DRAM controller
    - DRAM controllers designed to maximize DRAM data throughput
    - DRAM scheduling policies are unfair to some applications
        - row hit first: applications with high row buffer locality
        - oldest first: ufairly priotizes memory-intensive applications

- Mmeory performance hog:
    - T0: Stream:
        - sequential memory access
        - very high row buffer locality (96% hit rate)
        - memory intensive
    ```c
    for (int j=0;j<N;j++){
        index = j*linesize;
        A[index] = B[index];
    }
    ```
    - T1: Random:
        - random memory access
        - very low row buffer locality
        - similarly memory intensive
     ```c
    for (int j=0;j<N;j++){
        index = rand();
        A[index] = B[index];
    }
    ```   
    - the result is that the memory request buffer will service all the T0 memory requests because of this unfairness instead of T1

- DRAM refresh
    - A DRAM cell consists of a capacitor and an access transistor
    - it stores data in terms of charge in the capacitor
    - a DRAM chip consists of (10s of 1000s of) rows of such cells
    - DRAM capacitor charge leaks over time
    - memory controller need to refresh each row periodically to restore charge
        - activate each row every N ms
        - typicla N = 64ms
    - downsides of refresh:
        - energy consumption: each refresh requires energy
        - performance degradation: DRAM rank/bank unavailable while refreshed
        - QoS predictability impact: (Long) pause times during refreshed
        - refresh rate limits DRAM capacity scaling
    

- An example:
    - observation: Most DRAM rows can be refreshed much less often without losing data
    - key idea:
        - refresh rows containing weak cells more frequently, other rows less frequently
        - 1. profiling: profile retention time of all rows
        - 2. binning: store rows into bins by retention time in memory controller
        - 3. refreshing: memory controller refreshes rows in different bins at different rates

- DRAM Row Hammer (DRAM disturbance errors)
    - to access some data, you need to apply high voltage to a row
    - then to access another row you need to apply a low voltage to close the row
    - if you do this over and over again
        - repeatedly opening and closing a row enough times within a refresh interval induces disturbance errors in adjacent rows in most real DRAM chops you can buy today
        - youre disturbing the words bits in the victim rows, it affects the charge in the adjacent rows
        - every refresh cycle, you are accessing the aggresor row 
        - you can induce errors in real systems
            - a real reliability and security issue 
            - we can induce as many as ten million disturbance errors
    
    - How do we solve this problem:
        - do business as usual but better: improve circuit and device technology such that disturbance does not happen
        - tolerate it: make DRAM and controllers more intelligent to proactively fix the errors
        - eliminate or minimize it: replace DRAM with a different 