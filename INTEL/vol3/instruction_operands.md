Instruction Operands
---
- instruction format
	- label: mnemonic argument1, argument2, argument3
	- label is an identifier followed by a colon
	- mnemonic is a reserved name for a class of instrction opcodes which have the same function
	- operands argument{1,2,3} are optional
		- can take form of literals or identifiers for data items
		- operand identifiers are either reserved names of registers or are assumed to be assigned to data items declared elsewhere
	- example: 
		- LOADREG: MOV EAX, SUBTOTAL 

segmented addressing
---
- byte addressing: an address location is a byte
- segment addressing has segment-number : segment-offset
	- segment-number is a 4-bit hex number shifted left 1 hex bit to make a 5-bit hex number
	- segment-offset is a 4-bit hex number not shifted
	- full logical address is therefore the 20 binary bit sum of the two numbers
	- in code, the shifted segment-number resides in segment-register, so the register is specified instead of the actual segment address
	- segment registers:
		- SS : denotes stack segment pointer
		- DD : denotes data segment pointer
		- CS : denotes code/text segment pointer
		- ES : extra segment pointer used for programmer's discretion
		- FS : another pointer-register like ES
		- GS : another pointer-register like ES

Exceptions
---
- exception is an event that usually occurs when an instruction causes an error like divide by zero or accessing nonexistent memory
	- can also occur with breakpoints in debugging mode
- some exceptions provide error codes
	- reports additional info about the error
- example of exception and error code notation
	- #PF(fault code)
		- refers to page-fault where an error code names the type of page fault
	- exceptions are not always able to give an accurate code, so error code gives zero
		- #GP(0) is a general-protection exception

More information
---
- http://developer.intel.com/design/processor/
	- data sheets, application notes, manuals, papers, specification updates


