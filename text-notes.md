#Chapter 4, The Processor

>**In a major matter, no details are small** 
>- French proverb

##4.1 Introduction
In this chapter, we create the **datapath** and **control unit**, for *two different* implementations of the MIPS instruction set.

We begin by explaining the principles and techniques used in implementing a processor.  Then we build a simple version of the processor for the MIPS-like ISA.  Afterwards, we discuss a **pipelined** MIPS implementation.

Specifically:

- For basic pipelining: 4.1, 4.5
- For recend trends: 4.10, 4.11
- For instruction level parallelism: 4.12
- For processor & its performance: 4.3,4.4,4.6
- For building a processor: 4.2,4.74.8,4.9
- For modern hardware design: 4.13

###A Basic MIPS Implementation
We will implement a subset of the core MIPS instruction set,

- Memory-reference instructions *load word* (`lw`) and *store word* (`sw`)
- Arithmetic-logical instructions `add`, `sub`, `AND`, `OR`, and `slt`
- Some branching instructions, `beq`, *jump* (`j`), which we'll add last

Note that this subset does **not** include all integer instructions (multiply, divide, shift), or any floating-point instructions.

From examining this implementation, we see

- How the ISA influences the physical architecture.  
- How different implementation techniques affect the clock rate and CPI
- How the implementation exemplifies several key design principles, like how *simplicity favors regularity*

Note that the concepts used to implement this subset can be used to design a large spectrum of computers.
 
###An Overview of the Implementation

From the core MIPS instructions in Chapter 2, we have 

- integer arithmetic-logic instructions
- memory-reference instructions,
- branch instructions.

Note that there are a good amount of similarities between the three different types.  In particular, the first two instructions are the same:

1. Send the program counter (PC) to the memory that contains the code, and fetch the instruction from that memory.
2. Read one or two registers, using the fields of the instruction to select the registers to read.  We only read one register for the `lw` instruction, but for most o ther instructions we requires reading two registers.

There are also other similarities.  
>Note: similarities can be abstracted away in code.  Why not do the same for hardware? 
> ⇒ simplicity leads to regularity.

For example, all instructions, except for jumps, use the ALU after reading the registers.  

- The memory-reference instructions use the ALU for address calculation.
- The arithmetic-logic instructions use the ALU for the operation execution.
- The branch instructions use the ALU for 