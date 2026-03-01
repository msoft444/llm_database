---
source_pdf: rp2350-datasheet-10.pdf
repository: llm_database
chapter: Chapter 6. Power
section: 6.5.5. Full memory power down
pages: 491-492
type: technical_spec
generated_at: 2026-03-01T12:20:14.494106+00:00
---

# 6.5.5. Full memory power down

6.5.5. Full memory power down

RP2350 can completely power down its internal SRAM. Unlike the memory periphery power down described in Section

6.5.4, this completely disconnects the SRAM from the power supply, reducing static power to near zero.

Contents are lost when fully powering down memories. When you power memories up again following a power down,

6.5. Power reduction strategies
490

RP2350 Datasheet

the contents is completely undefined.

There are three distinct SRAM power domains:

SRAM0

Contains main system SRAM for addresses 0x20000000 through 0x2003ffff (SRAM banks 0 through 3).

SRAM1

Contains main system SRAM for addresses 0x20040000 through 0x20081fff (SRAM banks 4 through 9).

XIP

Contains the XIP cache and the boot RAM.

The XIP power domain is always powered when the switched core domain is powered. The switched core domain is the

domain which includes all core logic, such as processors, bus fabric and peripherals. This means the memories in this

domain are always powered whenever software is running.

Besides powering memory down to save power, you can also leave memories powered up whilst powering down the

switched core domain. This retains program state in SRAM while eliminating static power dissipation in core logic.

For more information see:

• Chapter 4 for a list of RP2350 memory resources, including main system SRAM, the XIP cache and boot RAM
• Section 6.2.1 for the definition of core power domains, including the memory power domains enumerated above
• Section 6.2.2 for the list of supported memory power states
• Section 6.2.3 for information on initiating power state transitions to power memories up or down
• Section 14.9.7.2 for typical power consumption in low-power states including memory power down
