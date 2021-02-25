# Instructions
### This doc contains how instructions operate in SIMDISCA.

<br>

## Overview
SIMDISCA is a 32-bit, little-endian, reduced-instruction-set computer architecture. SIMDISCA is an intuitive architecture to program.

SIMDISCA, externally, to the programmer, can only move data between registers and to/fro memory. However, using an assortment of 28 direct-access registers, and a heavy state-machine model, the architecture is fully Turing-complete.

## Opcodes
SIMDISCA only has one "opcode", per se.

In SIMDISCA, the function of the instructions come in the operands rather than the opcodes.

This is the structure of an 8-byte long SIMDISCA instruction:
```
F0 .. .. .. .. .. .. 0F
```
In this case, the `F0` byte represents an "opcode", if you will. `F0` is constant, however, and will always start an instruction.
`0F` is a magic byte used for padding instructions to align them to 8 bytes. In conjunction, `F0` and `0F` are used to validate instructions and ensure the machine isn't running garbage.

## Operands
Refer to the model from before:
```
F0 .. .. .. .. .. .. 0F
```
The 6 variable bytes in between are used to pass operands.

The only "instruction" available in SIMDISCA is a `mov`/`mv`.

You may only move between registers, or from register between memory.

The first operand is a type byte, the 1st byte in the instruction.

The second operand is also a type byte, the either the 2nd byte or the 6th byte in the instruction, depending on the previous operand. It may only be a value from `0x04`-`0x20`, only a register value, if the previous operand was not a register value. 

The first operand is used as the destination for data, and the second operand is used as the source.

The valid values for type bytes are:
 - `0x00`: immediate value: read the next 4 bytes in little-endian order as a value
 - `0x01`: byte from memory: read the next 4 bytes in little-endian order as an address
 - `0x02`: word from memory: read the next 4 bytes in little-endian order as an address
 - `0x03`: dword from memory: read the next 4 bytes in little-endian order as an address
 - `0x04`-`0x20`: [register](REGISTERS.md) destination
 - `0x21`: byte from memory: read the next byte as a register operand, then read three more bytes as an offset, and add the offset to the register's value and use the result as an address
 - `0x22`: word from memory: read the next byte as a register operand, then read three more bytes as an offset, and add the offset to the register's value and use the result as an address
 - `0x23`: dword from memory: read the next byte as a register operand, then read three more bytes as an offset, and add the offset to the register's value and use the result as an address
 - `0x24`: byte from memory: read the next byte as a register operand, then read three more bytes as an offset, and sub the offset to the register's value and use the result as an address
 - `0x25`: word from memory: read the next byte as a register operand, then read three more bytes as an offset, and sub the offset to the register's value and use the result as an address
 - `0x26`: dword from memory: read the next byte as a register operand, then read three more bytes as an offset, and sub the offset to the register's value and use the result as an address

If the operand attempts to transfer between memory, an #IO exception will arise.
If the operand attempts to move an immediate value into memory, an #IO exception will arise.
If the operand is `0x21`-`0x23` and the next byte is not a register byte, an #IO exception will arise.
If any two operands combine that make no sense, an #IO exception will arise. 

The destination operand cannot be an immediate value. Doing so will raise an #IO exception.

Due to the register memory offsets, the machine has a "high memory area" of 16MiB after the 4GiB mark. See [this doc on the high memory area](HIGH-MEMORY-AREA.md) for more details on its limitations and what it should be used for.

If a negative offset attempts to address below 0, it will wraparound into the High Memory Area.

## Example instructions

`mv gra, grb`:
```
F0 04 05 00 00 00 00 0F
```
`mv gra, 0xfba000`:
```
F0 04 00 00 A0 FB 00 0F
```
`mv d gra, grb`:
```
F0 23 04 00 00 00 05 0F
```
`mv d 0x59c000, grb`:
```
F0 03 00 C0 59 00 05 0F
```

Finally, the most complex possible:

`mv d gra off 0xac7000, grb`
```
F0 23 04 00 70 AC 05 0F
```