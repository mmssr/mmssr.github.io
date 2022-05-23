---
layout: post
title: "Reverse Engineering for Noobs"
date: 2022-05-17
---

<h2>Introduction</h2>  
Below is a presentation I am working on to introduce people to RE. Work in progress :)  
<hr>  

![Welcome](/assets/b-sides-knox.PNG)  
 
<h3>What is RE</h3>  
At a high level, reverse engineering is learning how something works without knowledge of it. With software, our goal is to understand the source code of a binary which we do not have the source code to. As a question: "what does this do, and how does it do it?" Our desired outcomes may come in varying degrees, from essentially re-creating software to just understanding what certain functions are performing. A large amount of analysis can be done by simply running and observing software, however this document is primarily about static analysis and examining the executable machine code. We gain a detailed view of the binary because, simply put, we are looking at the exact instructions which are running on a CPU.  

<h3>Why RE/Practical reasons</h3>  
You might be thinking that this sounds like a tedious and daunting exercise. This is largely correct, however there are a few reasons to do so anyways. You may have a malware sample found on a network which is sending encrypted outbound internet traffic and need to figure out what the traffic was. In this situation, dynamic analysis may not give you any insight into the payload data. Alternatively, de-compiling may not truly recreate the source code. While useful, de-compilers contain an element of guesswork. Therefore, you may need to reverse engineer the binary. Additionally, you will see reverse engineering used in context with game hacking, DRM removal, CTFs, etc.  

<h3>What is the goal of this document</h3>  
In the short term, my goal is to introduce you to reverse engineering both conceptually and practically. Due to the abstract and esoteric nature of RE as a field, most people are entirely unaware of its existence, much less exposed to an entry point. Personally, the most difficult period I had was finding training, gaining a methodology, and learning where to start. While there are fantastic resources for doing so, finding them can be tough. Reversing is an extremely deep field due to its ability to be applied to just about anything electronic, so consider this effort to be a diving board. Furthermore, it is a fantastic introduction to low level computing in general, where you can leverage your knowledge for things like binary golf, exploit development, modern assembly development, virus development, and more. In the long term, my goal is to introduce a mindset. Most of my RE experience at the time of writing has been in CTFs. Often you are presented with bizarre problems you are unprepared for, with little insight as to proceed. In order to solve these sorts of challenges, the following methodology becomes critical:  
- Failing attempts must be thought of iterative learning  
- "I don't understand X" must become "How do I learn more about X"  
- "Y is too complicated" must become "How do I break Y into smaller steps"  

<h2>Technical Prerequisites</h2>  
<h3>Executables (aka Binaries)</h3>  
At a high level, here is what is going on: We have instructions for humans but need to make machine code that a computer can execute. With a compiled language, higher level code such C is parsed and compiled into machine code.  
Compiling can be thought of in a few steps:  
- A compiler takes the source code and converts it into machine language, such as an object file of your source code.  
- A linker combines this object file with other pre-compiled object files so that your symbols (for example: identifiers for functions in libraries, global variables, etc) can be defined. When writing code you will often use functions you did not actually define within your source code. The linker takes your undefined symbols and adds the correct addresses to these instructions; outputting your executable file. As an example, imagine say you have an source code calling an exported DLL function. When that source code is compiled, the function call generates an external function reference (since the DLL is not a part of your source code.) The linker then adds the information necessary for the reference to actually find this DLL code when the process is started.  

![compiling](/assets/b-sides-knox3.PNG)  

The compiled executable looks like gibberish to us compared to the previous source code. However, this won't stop us from our goals.  
- While we may describe the executable as "binary," typically represent it in a shorter format, hex. An example of hex: B8 05 00 00 00.  
- Assembly is just translating this directly to a more human readable format. "B8 05 00 00 00" directly corresponds to "MOV EAX, 05" (move the value 05 into register EAX). B8 directly corresponds to the physical circuits for this instruction on the processor. This performs a MOV operation that loads a value into the physical 32-bit memory register on the processor that we are referring to as EAX. The latter half of the hex instruction for B8 is "05 00 00 00". There are 32 bits allocated for this value, but we only need a little of that to represent our value, hence the zeroes.  
- Also note, the 05 is stored on the far left vs the far right. This is due to something referred to as "endianness," which you will want to learn more about later, but don't worry about that right now. Just know that if the byte on the far left would represent the least significant bit it would be referred to as little-endian, and if the byte on the far left represented the most significant bit, it would be referred to as big-endian. Were we to represent this as a big-endian value, the instruction in hex would be "B8 00 00 00 05." PE (.exe) files will always be little-endian, but know that other formats such as ELF may not be.  

<h3>What happens when a program is run?</h3>  
- The program is assigned RAM, and the code/data it manipulates are loaded into RAM from storage.  
- An instruction is read from RAM by the CPU control unit.  
- The instruction is executed by the CPU ALU using inputs from user or registers.  
- The output is stored in registers or sent to a device.  
- Repeat these steps until the program is done.  

![CPU](/assets/b-sides-knox4.PNG)      
        
<h3>x86 Assembly Overview (32 bit)</h3>  
Instructions follow the format of:  

```  

instruction dest_operand, source_operand  

```  

<h4>A Few Common Instructions</h4>  
Movement of data  
- MOV  
- LEA  

Math/logic  
- ADD  
- SUB  
- OR  
- XOR  
- AND  

Control flow  
- CMP  
- JMP  
- PUSH  
- POP  
- CALL  
- RET  

<h4>General Registers</h4>  
Think of registers as variables within x86. This is not an exhaustive list of every register which can be used, however the vast majority of the time you will be seeing these registers.  
                
![Registers](/assets/b-sides-knox2.PNG)  

Data Registers  
- EAX: Accumulator, often for math and return values.  
- EBX: Base, often holds function params and indexed addresses.  
- ECX: Count, for loop counts (programmers: this is "i") and function params.  
- EDX: Data, general use and function params.  
- Internally labeled boxes are subregisters which can be used.   

Index Registers  
- ESI: Source Index, often stores constants or used as a pointer.  
- EDI: Destination Index, often a destination for data or used as a pointer.  

Pointers  
- Points to a memory location.  
- ESP: Stores a pointer to top of stack.  
- EBP: Stores a pointer to base of current stack frame.  

Flags  
Certain instructions change flags, which are represented as simple "1" or "0" on a flag register. For example, if we perform an "add" between two numbers, we may have a few outcomes that we need to be aware of. Let's say we compare two values to see if they are equal with CMP EDX, EAX. The "zero flag" will either be set to 1 if they are equal, or 0 if they are not. An instruction like JZ 0x40154b that jumps to memory location 0x40154b if the comparison equaled to zero would check this flag. Think of these as boolean variables. There are a handful of flags to learn, but a few to get you started are:  
- ZF, "zero flag," set if result is 0.  
- SF, "sign flag," set if a result is negative.  
- OF, "overflow flag," set if a result was larger than a register can hold. Let us say AL and DL are set to 0xFF. ADD AL, DL would return a 9 bit character and therefore overflow the 8 bit register.  


<h4>The Call Stack</h4>  
purposes //todo  
parameters //todo   
local data //todo   
Lets talk about pointers relative to the stack (ESP, EBP) //todo   
EBP always points to the base of the stack frame  
ESP always points to top of stack  
- Increases as values are popped off of the stack. This is so you will eventually be set to your base pointer.   
- Decreases as values are added to the stack. This is so you cannot go into the negatives where memory addresses no longer exit.  

How it works  
- When a function is called, the current EBP is pushed onto the stack.  
- EBP is then assigned the value of ESP; EBP is now the base pointer to access vars and params.  
- As function runs, ESP will change as needed, EBP remains static.  
- As function terminates, ESP is set to EBP.  
- Stack pointer is now set to top of stack to equal the base pointer. This frees memory allocated on the stack for local vars.  
- EBP is now popped from the stack (remember, first in last out,) and EBP can be used again as base pointer. In simpler terms, stack frames contain the EBP value of the functions caller (step 1.) Now we can use EBP register as this function's base pointer.  

<h4>Addressing modes</h4>  
We have a few ways to address things.  
- Register addressing, aka using the direct register references.  
- Immediate addressing, where we give an immediate value.  
- Memory addressing, where we use a memory address. Direct memory addressing is when we reference the effective address of a memory location directly, eg "JMP 0x401438" jumps directly to memory address 0x401438. Indirect memory addressing, eg "JMP [EAX]" jumps directly to the memory address stored in EAX.  

Addressing examples:  
- Register to register (eg, MOV EDX, EAX copies data in EAX to EDX).  
- Register to memory (eg, MOV [EDX], EAX copies data in EAX to memory address in EDX).  
- Memory to register (eg, MOV EDX, [EAX] copies the data at memory address in EAX to EDX).  
- Immediate to register (eg, MOVe EDX, 0x05 copies hex value 5 to EDX).  
- Immediate to memory (eg, MOV [EDX], 0x05 copies hex value 5 to memory address in EDX).  

Note there is no "to immediate." This is because you can store things at a memory location, or within a register, but you cannot store things at decimal value "10" for example. Spoken as a analogy, you can store apples in a bucket, you can store apples at a street address for a bucket, but you can't store apples at an apple.  

<h4>File formats</h4>  
General focuses  
- header  
- code (.code, duh)  
- strings (.rdata)  
- imported functions (.idata)  

PE  
PE structure  
- DOS Header  
- DOS Stuf  
- COFF header  
- Data Directories  
- Section Table  

Memory Map  
- Stack  
- Heap  
- Program Image  
- DLLs  
- TEB  
- PEB  
- ELF  

<h3>Developing a Methodology</h3>  
- Checking the headers for our sections  
- Checking strings  
- Checking function imports   


