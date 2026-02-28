---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.5.6. Self-hosted debug
pages: 88-88
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 3.5.6. Self-hosted debug

arbitrate with core 1’s load/store port to access the system bus, but they are treated as distinct from core 1’s accesses
for the purpose of bus filtering (Section 10.6.2), which means it is possible to lock software out of a peripheral whilst
retaining debug access. Processor load/stores in Debug mode are also treated as debug accesses for the purpose of
bus filtering.
The DM is able to reset each core individually using the dmcontrol.hartreset control. This resets only the selected
processor. The dmcontrol.ndmreset resets both processors only, which is the minimum requirement in the RISC-V debug
specification. A full system reset, which includes the DM, can be performed using the SYSRESETREQ control in the SW-
DP, a switched core domain reset configured in POWMAN and initiated by the watchdog, or any full-system reset such
as the RUN pin. A PSM reset initiated by the watchdog can reset almost all system-level hardware except for the DM,
but note that the DM becomes momentarily inaccessible whilst the system clock’s clock generator is reset, which is the
reason for dmcontrol.ndmreset resetting the processors only.
For details on the processor side of RISC-V debug, see Section 3.8.5. See also the Hazard3 source code at
github.com/Wren6991/Hazard3, which includes the DM implementation under the hdl/debug/dm/ directory.
3.5.4. Debug power domains
The SW-DP and the RP-AP are in the always-on power domain. This means they are available even when the system is in
its lowest-power state, with the switched core domain (which includes the processors) fully powered down.
The remainder of the debug hardware is in the switched core domain. This is the same domain as the processors and
system peripherals.
Setting the CDBGPWRUPREQ bit in the SW-DP’s CTRL/STAT register will force a power up of the switched core domain,
making the remaining debug hardware available. This power up takes some time, as it is sequenced by the 32 kHz low-
power oscillator (Section 8.4), so the CDBGPWRUPACK bit must be polled to wait for the system to power up before
attempting to access any APs other than the RP-AP. See Arm’s ADIv6 specification for the SW-DP’s register listing.
Note that the RP-AP is accessible without asserting CDBGPWRUPREQ, as it is always powered.
3.5.5. Software control of SWD pins
The DBGFORCE register in SYSCFG can be used to detach the SW-DP from the external debug pads, and instead bitbang
the internal SWD signals directly from software. This is intended for a debug probe running on one core being used to
debug the other core. For other use cases it is generally cleaner to use the self-hosted debug access to interface with
the APs directly from the system bus.
3.5.6. Self-hosted debug
All APs shown in Figure 10, except for the RP-AP, have direct memory-mapped access from the system bus. This is
known as self-hosted debug, because with care it allows running a debug host (i.e. a debugger) directly on-system. It
can also be used to access the trace hardware, which can be used for self-hosted trace using the trace DMA FIFO. By
default only Secure access is permitted, as the processor debug presents an opportunity for Non-secure code to
interfere with the Secure context and/or perform Secure bus accesses.
The self-hosted debug window starts at address 0x40140000 (CORESIGHT_PERIPH_BASE). The offsets of the APs within
this window are the same as the APs' addresses when accessed from the SW-DP.
Because of the blocking nature of the AHB-AP’s DRW register, and its interactions with the Cortex-M33’s arbitration of
AHB-AP accesses with load/stores, certain accesses have potential to cause bus lockup due to circular bus stall
dependencies. In particular, cores may not access their own AHB-APs through the self-hosted debug window, and AHB-
APs may not access AHB-APs through the self-hosted debug window — attempting to do so will immediately return a
bus fault. To reduce the opportunities for deadlock, a full APB crossbar is used to connect the SW-DP and the self-
hosted debug port to the APs, so that for example self-hosted use of the Arm trace hardware will not interfere with an
external debugger attaching via the AHB-APs.
RP2350 Datasheet
3.5. Debug
87

