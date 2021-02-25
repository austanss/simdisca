# The High Memory Area

## Explanation
### Register Offset Addresses
Take the following code:
```
mv gra, 0xffffffff
mv b gra off 0xabd000, 0xbe
```
In other architectures, like x86, this would simply wrap around, and you would access `0xabd000`.

But in 32-bit machines, memory is of the essence. 32-bit addressing only allows you to address 4GiB of memory. Making space for commodities like MMIO just eats at the amount of RAM you can address.

As well, when offsetting registers by only a 24-bit value, wrap around is not as useful as it is in x86.

So, in SIMDISCA, we re-invented the High Memory Area. 

Used in 8086 terminology, it refers to the 64KiB of addressable memory that lies just past the 1MiB mark.

However, in SIMDISCA, it refers to the 16MiB of address space that can be accessed by offsetting 32-bit registers by a 24-bit address.

Take the above code. When you address memory with that value, you are addressing the value at `0x100abcfff`.

The maximum address you can offset a register to is `0x100fffffe`.

## Limitations
### Pointer Registers
No pointer register can point to any memory in the High Memory Area. All registers are 32-bits wide, and any address pointing to the High Memory Area is 33-bits wide.

### Executablility
You cannot execute code in the High Memory Area.
Due to pointer registers being 32-bits wide, as mentioned above, MRP is not able to point to the High Memory Area.

## Usage
The High Memory Area cannot be used for stack or code, so this area should not contain RAM. Instead, this area can and should be used for MMIO. MMIO should **always** stay inside the High Memory Area. This allows for MMIO to have its own space, not shadowing usable RAM.