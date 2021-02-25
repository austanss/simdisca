# Registers
This doc contains a comprehensive list of registers and their associated `mv` operands.

## General Registers
These registers are general-purpose, and have no special use.

 - GRA: `0x04`: general register A: general-purpose use
 - GRB: `0x05`: general register B: general-purpose use
 - GRC: `0x06`: general register C: general-purpose use
 - GRD: `0x07`: general register D: general-purpose use
 - GRE: `0x08`: general register E: general-purpose use
 - GRF: `0x09`: general register F: general-purpose use

## Operative Registers
These registers are used for operations.

### Operands:
**General Operands**:
 - ORL: `0x0A`: operation register left: holds the value of the left-hand operand in an operation
 - ORR: `0x0B`: operation register right: holds the value of the right-hand operand in an operation

**Conditional Operands**:
 - NRL: `0x0C`: conditional register left: holds the value that NRO will hold if ORL != ORR
 - NRR: `0x0D`: conditional register right: holds the value that NRO will hold if ORL == ORR

### Outputs:

**Arithmetic**:
 - ARA: `0x0E`: arithmetic register addition: holds the sum of the two operation registers, `0xFFFFFFFF` if overflown
 - ARS: `0x0F`: arithmetic register subtraction: holds the difference of the two operation registers, will wrap-around, unsigned
 - ARM: `0x10`: arithmetic register multiplication: holds the product of the two operation registers, `0xFFFFFFFF` if overflown
 - ARD: `0x11`: arithmetic register division: holds the quotient of the two operation registers, exception #MF when accessed if either register is 0
 - ARO: `0x12`: arithmetic register modulo: holds the remainder when dividing two operation registers, exception #MF when accessed if either register is 0

**Bitwise**:
 - BRA: `0x13`: bitwise register AND: holds the result of a bitwise AND on ORL, where ORR is a mask
 - BRO: `0x14`: bitwise register OR: holds the result of a bitwise OR on ORL, where ORR is a mask
 - BRX: `0x15`: bitwise register XOR: holds the result of a bitwise XOR on ORL, where ORR is a mask
 - BRN: `0x16`: bitwise register NOT: holds the result of a bitwise NOT on ORL, ORR is ignored

**Bitshift**:
 - SRL: `0x17`: bitshift register left: holds the result of a left bitshift on ORL, where ORR is the number of bits to shift
 - SRR: `0x18`: bitshift register right: holds the result of a right bitshift on ORL, where ORR is the number of bits to shift

**Conditional**:
 - NRO: `0x19`: holds either NRL or NRR depending on if ORL == ORR, if not, it holds NRL, if so, it holds NRR
## Pointer Registers

**Stack**:
 - PRS: `0x1A`: pointer register stack: holds the pointer to a "stack" (no push/pop instructions so it's a bit more complicated)
 - PRB: `0x1B`: pointer register [stack] base: holds the pointer to the stack base, available for stack frame support

## CPU Registers
 - MRF: `0x1C`: Machine register flags: holds bit flags about certain CPU-related boolean values, see [the CPU flags doc](CPU-FLAGS.md) for more info
 - MRI: `0x1D`: Machine register interrupt [vector table pointer]: holds the pointer to the CPU interrupt vector table, see [the interrupts doc](INTERRUPTS.md) for more info
 - MRE: `0x1E`: Machine register enabled (interrupt flags): holds bit flags on the status of whether certain interrupts are allowed to occur, see [the interrupts doc](INTERRUPTS.md) for more info
 - MRN: `0x1F`: Machine register number (associated with interrupt to trigger): triggers an interrupt with the value of itself when it is modified, exception #IO when value > 31
 - MRP: `0x20`: Machine register program (counter): the pointer to the current executing instruction's opcode, increments by 8 after executing an instruction