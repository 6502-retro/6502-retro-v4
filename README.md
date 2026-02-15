# 6502-Retro! V4.3

![6502-Retro-V4.3](./hardware/6502-retro-v4.3.png)

Continuing on from the 6502-Retro-V3-Banked SBC, this board now includes
additional IO in the form of a SPI bus carrying the SDCard interface and 2
additional SPI select lines.  There is also an additional 65C22 VIA with all of
its handshaking and port A and B pins brought out to a pin header.

Another optimization made relates to the RAM and ROM logic.  The RAM Bank is
controlled by the bottom 6 bits of the bank latch with the 7th bit used to
select between two rom banks and the 8th bit used to disable the ROM thus
enabling RAM in its place.

- Schematic: [6502-retro-v4.3.pdf](./hardware/6502-retro-v4.3.pdf)

## Memory

### Memory Bank Latch

Memory map is decoded with an ATF22V10 PLD.  Equations for which are found
here: [6502retrov4mem.pld](./pld/memdec/6502retrov4mem.pld)

- MSB of RAM BANK LATCH is ROMSW.
- PLD enables ROM (ROMEN=0) when ROMSW=0.
- PLD disables ROM(ROMEN=1) when ROMSW=1.
- RAM banks are driven by the lower 6bits giving 64 x 8k banks
- ROMSEL (bit 6) selects between the lower and upper 32k of ROM

### Memory Map

| Address | Description                                 |
| ------- | ------------------------------------------- |
| 0x0000  | FIXED RAM START/ ZERO PAGE                  |
| 0x0100  | HW STACK                                    |
| 0x0200  | FIXED USER RAM START                        |
| 0xBEFF  | FIXED USER RAM RAM END                      |
| 0xBF00  | IO- BANK LATCH (MSB=ROMSWITCH) (banks 0-63) |
| 0xBF10  | IO- ACIA/ UART                              |
| 0xBF20  | IO- VIA1 / SPI / Some IO                    |
| 0xBF30  | IO- VIA2 / Peripheral IO                    |
| 0xBF40  | IO- VDP/ TMS9918A Clone                     |
| 0xBF50  | IO- JSEN/ Joy Stick                         |
| 0xBF60  | IO- EXPANSION 1                             |
| 0xBF70  | IO- EXPANSION 2                             |
| 0xBF80- | IOCSH - On Expansion Bus                    |
| 0xBFF0  |                                             |
| 0xC000  | HIGH/ BANKED RAM/ 64 x 8kb banks ram        |
| 0xE000  | ROM/ 8KB ROM                                |
| 0xFFFA  | VECTORS/ HW vectors                         |
| 0xFFFF  | END OF ALL MEMORY                           |

## IRQ

There are 6 possible IRQ sources which are combined to a single /IRQ signal to
the CPU.  For convenience, a 16V8 GAL was used to perform this function.  The
equations for which are found here:
[6502-retro-v4-irq.pld](./pld/irq/6502-retro-v4-irq.pld)

This is not a priority encoder.

## License

Copyright © 2026 David Latham

This source describes hardware settled by the CERN-OHL-S v2. Licensed under the
CERN-OHL-S v2.0 or later. You may redistribute and modify this source and make
products using it under the terms of the CERN-OHL-S v2.0
[https://ohwr.org/cern_ohl_s_v2.txt](./LICENSE.md).

## Revision History

- v4.0 - Initial revision
- v4.1 - Fix SN76489 Audio Chip.  /SNWE must be driven by VIA, SNREADY requires
  2.2k pullup.
- v4.2 - Fix Power orientation on USB-C Adapter
- V4.3 - Larger board, memory decoder includes A7 address line, two expansion
  slots, pullups on expansion slot IRQ lines.

<!-- vim: set tw=80 cc=80: -->
