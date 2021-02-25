# Exceptions
When something goes wrong, the machine needs to notify the program, so that the program can attempt to resolve the issue.

Common methods of doing this include exceptions, interrupts that signal a possible disaster when raised. 

## All Standard Exceptions

 - \#IO: `0x0`: invalid operand: raised when an operand is supplied that is not compatible in the context of the other operands
 - \#MF: `0x1`: math fault: raised when there was an error performing an arithmetic operation
 - \#AF: `0x2`: addressing fault: raised when the machine attempted to address RAM/MMIO that was not available
 - \#GF: `0x3`: general fault: raised when the machine encounters an internal error. irrecoverable.
 - \#PF: `0x4`: program fault: raised by a program when an error occurs that is irrecoverable.

## Handling
It is generally ill-advised to mask exception interrupts. Doing so will allow the machine to run in error, thus allowing UB to flow on in, and possibly destroy hardware. 