**This manual focuses on protected mode.  Study in this context.**

System Architecture Overview
===
**IA-32 provides:**
- Memory management
- Protection of software modules
- Multitasking
- Exception and interrupt handling
- Multiprocessing
- Cache Management
- Hardware resource and power management
- debugging and performance monitoring

These services are often controlled by system registers

Overview
========
*Block diagram of control architecture is on page 41, AKA '2-2 Vol 3'*
- a copy of this page should be kept available for reference when learning this stuff


Global and Local Descriptor Tables
---
- in protected mode, all memory access are through the GDT or (optionally) LDT which contain segment descriptors
	- segment descriptor provides base address and access rights, type, and usage info for segment
	- segment descriptors have a segment selector associated with it, which provides:
		- an index into the GDT or LDT to the descriptor
		- a global/local flag which determines whether it points to GDT or LDT
		- access rights info 
- accessing a byte in a segment
	- segment selector and offset are given
		- segment selector gets us to the segment descriptor
		- processor then gets the base address of the segment in the linear address space
		- offset gives us the location of the byte relative to teh base address
	- this can be used to access any valid code, data, or stack segment in the GDT or LDT with current privilege level (CPL) restrictions
		- CPL is the protection level of the currently executing code segment
	- linear address is NOT a logical address.  Linear address involves further translation into physical address
- linear address of the base of the GDT is contained in the GDT Register (GDTR) 
- linear address of the base of the LDT is contained in the LDT Register (LDTR)
 

System Segments, Segment Descriptors, and Gates
---
- execution environment of a program uses the code, stack, and data segments mentioned previously
- system environment uses two system segments
	- task-state segment (TSS)
	- LDT
		- GDT is not because it's not accessed by segment selector and segment descriptor.  read on
	- each of these have a segment descriptor defined for it. continue to read on
- gates
	- special descriptors which provide protected gateways to system procedures and handlers that operate at different privilege levels than application programs and most procedures
	- call gate
		- seem to act as a way to call functions at a higher privilege level, 
		- also facilitate transitions between 16-bit and 32-bit code segments, and vice versa
	- interrupt gate
	- trap gate
	- task gate
		- 'similar to a call gate, but it provides access (through a segment selector) to a TSS rather than a code segment'


Task-State Segments and Task Gates
---
- a TSS defines state of the execution environment for a task
	- includes state of general-purpose registers, segmnet registers, EFLAGS register, EIP register
	- also includes segment-selectors and stack-pointers for stack segments for privilege levels {0,1,2}
	- also includes segment selector for the LDT associated with the task, and the page-table base address
- task
	- all program execution in protected mode occurs in the context of a 'task' which is called the current task
	- segment selector for the current task's TSS is stored in the task register
	- switching to a task
		- simplest method is to make a call or JMP to a task
		- processor performs:
			- store the state of the current task in the current TSS
			- load the task register with the segment selector for the new task
			- access the new TSS through a segment descriptor in the GDT
			- load the state of the new task
				- general purpose registers
				- segment registers
				- LDTR
				- CR3 (control register 3)
				- EFLAGS
				- EIP register
			- begins execution of the new task

Interrupt and Exception Handling
---
- external interrupts, software interrupts, and exceptions are handled through the Interrupt Descriptor Table (IDT)
- IDT
	- contains a collection of gate descriptors which provide access to interrupt and exception handlers
		- gate descriptors in IDT can be of 3 types
			- interrupt-gate descriptor
			- trap-gate descriptor
			- task-gate descriptor
	- IDT is not a segment
	- linear address of the base of the IDT is in the IDT Register (IDTR)
	- to access interrupt or exception handler, processor must receive an interrupt vector (number) from internal hardware, an external interrupt controller, or from software by the INT, INTO, INT3, our BOUND instructions
		- interrupt vector provides an index into IDT to a gate descriptor
		- if descriptor is an interrupt gate or a trap gate, the associated handler is accessed similarly to calling a procedure through a call-gate
		- if descriptor is a task-gate, handler is accessed through a task switch

Memory Management
---
- system architecture supports either direct physical addressing of memory or virtual memory through paging.
	- physical addressing
		- linear address is treated as a physical address
	- paging
		- all the code, data, stack, and system segments and the GDT and IDT can be paged, with only most recently accessed pages being held in physical memory
		- location of pages or page frames in physical memory
			- stored in 2 types of system data structures; both are in physical memory
				- page directory
				- set of page tables


System-Registers
---

Other System Resources
---

Modes of Operation
=======

System Flags and Fields in the EFLAGS Register
======

Memory-Management Registers
======

Control Registers
======

System Instruction Summary
=====
