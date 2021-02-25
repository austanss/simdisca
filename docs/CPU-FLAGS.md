# CPU Flags
This doc contains information about CPU flags and what purpose they serve.

## The CPU flags register
The MRF register stores CPU flags. It's operand is `0x19`.

## All flags
Bits 31-3 are reserved.


Bit 0 represents the execution flag. When set, the CPU will execute normally. When unset, the CPU will halt.


Bit 1 represents the interrupt flag. When set, the CPU will accept all raised interrupts. When unset, all interrupts are ignored.

Bit 2 represents the ROM shadow disable flag. When unset, the 256KiB area of memory from `0xFFFC0000` up until the end of the normal memory area will be shadowed by the firmware ROM. When set, this shadow is cleared, allowing all of the normal memory area to be used for RAM.