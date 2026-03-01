---
source_pdf: rp2350-datasheet-10.pdf
repository: llm_database
chapter: Chapter 6. Power
section: 6.5.4. Memory periphery power down
pages: 491-491
type: technical_spec
generated_at: 2026-03-01T12:20:14.494106+00:00
---

# 6.5.4. Memory periphery power down

6.5.4. Memory periphery power down

The main system memories (SRAM0 → SRAM9, mapped to bus addresses 0x20000000 to 0x20081fff), as well as the USB

DPRAM, can be partially powered down via the MEMPOWERDOWN register in the SYSCFG registers (see Section

12.15.2). This powers down the analogue circuitry used to access the SRAM storage array (the periphery of the SRAM)

but the storage array itself remains powered. Memories retain their current contents, but cannot be accessed. Static

power is reduced.

CAUTION

Memories must not be accessed when powered down. Doing so can corrupt memory contents.

When powering a memory back up, a 20ns delay is required before accessing the memory again.

The XIP cache (see Section 4.4) can also be powered down, with CTRL.POWER_DOWN. The XIP hardware will not

generate cache accesses whilst the cache is powered down. Note that this is unlikely to produce a net power savings if

code continues to execute from XIP, due to the comparatively high voltages and switching capacitances of the external

QSPI bus.
