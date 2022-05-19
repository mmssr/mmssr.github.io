---
layout: post
title: "Reverse Engineering for Noobs"
date: 2022-05-17
---

<h2>Introduction</h2>  
Below is a presentation I am working on to introduce people to RE. Work in progress :)  
<hr>  

![Welcome](/assets/b-sides-knox.PNG)  

```  

High Level Outline  
1. Introduction  
    -What is RE  
        -At a high level, reverse engineering is learning how something works without knowledge of it. With software, our goal is to understand the source code of a binary which we do not have the source code to. As a question: "what does this do, and how does it do it?" Our desired outcomes may come in varying degrees, from essentially re-creating software to just understanding what certain functions are performing. A large amount of analysis can be done by simply running and observing software, however this document is primarily about static analysis and examining the executable machine code. We gain a detailed view of the binary because, simply put, we are looking at the exact instructions which are running on a CPU.  
    -Why RE/Practical reasons  
        -You might be thinking that this sounds like a tedious and daunting exercise. This is largely correct, however there are a few reasons to do so anyways. You may have a malware sample found on a network which is sending encrypted outbound internet traffic and need to figure out what the traffic was. In this situation, dynamic analysis may not give you any insight into the payload data. Alternatively, de-compiling may not truly recreate the source code. While useful, they contain an element of guesswork. Therefore, you may need to reverse engineer the binary.  
        -Additionally, you will see reverse engineering used in context with game hacking, DRM removal, CTFs, etc.  
    -What is our goal  
        -In the short term, my goal is to introduce you to reverse engineering both conceptually and practically. Due to the abstract and esoteric nature of RE as a field, most people are entirely unaware of its existence, much less exposed to an entry point. Personally, the most difficult period I had was finding training, gaining a methodology, and learning where to start. While there are fantastic resources for doing so, finding them can be tough. Reversing is an extremely deep field due to its ability to be applied to just about anything electronic, so consider this effort to be a diving board. Furthermore, it is a fantastic introduction to low level computing in general, where you can leverage your knowledge for things like binary golf, exploit development, modern assembly development, virus development, and more.  
        -In the long term, my goal is to introduce a mindset. Most of my RE experience at the time of writing has been in CTFs. Often you are presented with bizarre problems you are unprepared for, with little insight as to proceed. In order to solve these sorts of challenges, the following methodology becomes critical:  
            -Failing attempts must be thought of iterative learning  
            -"I dont understand X" must become "How do I learn more about X"  
            -"Y is too complicated" must become "How do I break Y into smaller steps"  

2. Technical Prerequisites  
    -Executables  
        -High level code compiled into machine code  
        -Machine code can be dissassembled into assembly code  
    -How does a CPU work when a program is run  
        -Instruction is read from main memory by control unit.  
        -Instruction is executed by ALU using inputs from user or registers.  
        -Output is stoed in registers or sent to a device.  
        -Repeat steps till program is done.  
    -x86 assembly overview (32 bit)  
        -instructions follow the format of:  
            instruction dest_operand, source_operand  
        -common instructions  
            -movement of data  
                -mov: copies data from one location to another  
                -lea: load effective address  
            -math  
                -add  
                -sub  
                -inc  
                -dec  
                -mul  
                -div  
            -logic  
                -or  
                -xor  
                -and  
                -not  
                -shr  
                -shl  
            -control flows  
                -test  
                -cmp  
                -jmp  
                -jcc  
                -jz  
            -stack  
                -push  
                -pop  
                -call  
                -ret  
            -reference:  
                -https://www.intel.com/content/dam/www/public/us/en/documents/manuals/64-ia-32-architectures-software-developer-vol-2a-manual.pdf  
                -https://www.intel.com/content/dam/www/public/us/en/documents/manuals/64-ia-32-architectures-software-developer-vol-2b-manual.pdf  
                -https://www.felixcloutier.com/x86/index.html  
                
```  

![Registers](/assets/b-sides-knox2.PNG)  

```  

        -General Registers
            -Data Registers  
                -Names are more historical than they are prescriptive.  
                -EAX: Accumulator, often for math and return values.  
                -EBX: Base, often holds function params and indexed addresses.  
                -ECX: Count, for loop counts (programmers: this is "i") and function params.  
                -EDX: Data, general use and function params.  
                -Internally labeled boxes are subregisters which can be used.   
            -Index Registers  
                -ESI: Source Index, often stores constants or used as a pointer.  
                -EDI: Destination Index, often a destination for data or used as a pointer.  
            -Pointers  
                -Points to a memory location  
                -ESP: Stores a pointer to top of stack.  
                -EBP: Stores a pointer to base of current stack frame.  
        -flags  
        -the call stack  
            -purposes  
            -parameters  
            -local data  
            -lets talk about pointers relative to the stack (ESP, EBP)  
                -EBP always points to the base of the stack  
                -ESP always points to top of stack  
                    -increases as values are popped off of the stack  
                    -decreases as values are added to the stack  
                    -Why is this? because you cannot go negative into the base pointer  
            -How it works  
                1. When a function is called, the current EBP is pushed onto the stack.  
                2. EBP is then assigned the value of ESP; EBP is now the base pointer to access vars and params.  
                3. As function runs, ESP will change as needed, EBP remains static.  
                4. As function terminates, ESP is set to EBP.   
                    -Stack pointer is now set to top of stack to equal the base pointer. This frees memory allocated on the stack for local vars.  
                5. EBP is now popped from the stack (remember, first in last out,) and EBP can be used again as base pointer.  
                6. In simpler terms, stack frames contain the ebp value of the functions caller (step 1.)  
                    -Now we can use ebp register as this function's base pointer.  
        -addressing modes  
        
    -File formats  
        -General focuses  
            -header  
            -code (.code, duh)  
            -strings (.rdata)  
            -imported functions(.idata)  
        -PE  
            -PE structure  
                -DOS Header  
                -DOS Stuf  
                -COFF header  
                -Data Directories  
                -Section Table  
            -Memory Map  
                -Stack  
                -Heap  
                -Program Image  
                -DLLs  
                -TEB  
                -PEB  
        -ELF  

3. Developing a Methodology  
    -Checking the headers for our sections  
    -Checking strings  
    -Checking function imports  

```  
