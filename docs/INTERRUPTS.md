# Interrupts
SIMDISCA supports numbered interrupts, from 0-31.

## The Interrupt Vector Table
The Interrupt Vector Table (IVT) is a 256-byte structure containing the pointers to handlers of an associated interrupt.

When the machine triggers an interrupt, it reads the value of MRN. It takes this value, multiplies it by 4, and the uses the sum of MRI and this new value to read the address of an interrupt handler in the IVT.

Afterwards, it sets MRN to zero, and proceeds to the interrupt procedure.

## Interrupt Registers

### MRI
MRI holds a pointer to the IVT. Specifically, it points to the 1st entry in the IVT, the address to the interrupt 0 handler.

### MRE
MRE holds bit masks on interrupts. When the machine gets a reqeuest to trigger an interrupt, it checks if the bit in MER associated with the interrupt is set. For example, if interrupt 31 is requested to be triggered, the CPU checks to see if bit 31 is set in MRE. If not, it does not continue to trigger the interrupt, and goes back to normal execution. If so, it continues to trigger the interrupt normally.

### MRF[1]
Bit 1 of MRF contains the interrupt flag. If this bit is not set, no interrupts are triggered at all.

## Interrupt Procedure

When the machine starts the interrupt procedure, it performs these following steps:

 1) Decrements the PRS register by 4.
 2) Places the current value of MRP at PRS.
 3) Sets MRP to the value from the IVT.
 4) Begins execution.