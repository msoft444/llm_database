---
source_pdf: rp2350-datasheet-9.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.5.1. Connecting to the SW-DP
pages: 86-87
type: technical_spec
generated_at: 2026-03-01T11:44:31.548839+00:00
---

# 3.5.1. Connecting to the SW-DP

3.5.1. Connecting to the SW-DP

The SW-DP defaults to the Dormant state at power-up or assertion of the external reset (RUN) pin. A Dormant-to-SWD

sequence must be issued before beginning SWD operations. See the Arm Debug Interface specification, version 6, for

details of Dormant/SWD state switching: https://developer.arm.com/documentation/ihi0074/latest/

After a power-on, the following sequence can be used to connect to the SW-DP:

1. At least 8 × SWCLK cycles with SWDIO high.

3.5. Debug
85

RP2350 Datasheet

2. The 128-bit Selection Alert sequence: 0x19bc0ea2, 0xe3ddafe9, 0x86852d95, 0x6209f392, LSB-first.

3. Four SWCLK cycles with SWDIO low.

4. SWD activation code sequence : 0x1a, LSB first.

5. At least 50 × SWCLK cycles with SWDIO high (line reset).

6. A DPIDR read to exit the Reset state

In order to wake up the system from a low power (P1.x) state, set the CDBGPWRUPREQ in the DP CTRL/STAT register,

then poll CDBGPWRUPACK in the same register until set. In low-power states, only the SW-DP and RP-AP are accessible,

as the remaining debug logic is unpowered.
