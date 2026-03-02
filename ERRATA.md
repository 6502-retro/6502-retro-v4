# ERRATA

## V4.3

Version of V4.3 of this board has some significant bugs as well as some lessor
but still concerning issues.

### BUGS (2026-03-03)

- Because the Atmel ATF22V10C outputs TTL logic levels, we need all the Logic
ICs to be TTL Level.  The LS variants are a bit too slow at 4MHz.  So HCT Logic
families are needed.
    - 74HCT273
    - 74HCT138
    - 74HCT244
    - 74HCT32

- Joystick Port is missing ground connections on pins 8 and 9.
- 74HCT244 has two enable pins (1 and 19) that need to be tied together and
linked to the Y5 on the 74HCT138 (U14)
- The SN76489A Data pins are connected to the VIA1-PORTB in the reverse order.
The code in tag v4.3 of the 6502-retro-boot and 6502-retro-os repos account for
this error by reversing the byte before sending to the chip.

### Concerning Issues

- There is significant noise on the audio.  A constant humming sound can be
heard.
- The IRQ processor when driven by the PICO9918 is set to also call an
interrupt service routine in the SN76489 library on the os which will turn off
the user LED when the counter reaches zero.
- The current limiting resistor on the RED Power LED is too low.  A stronger
470ohm or even 1k ohm is a better call.
- There is quite a significant ring on the phi2 signal.  Some form of signal
dampening should be applied to the clock signal.

