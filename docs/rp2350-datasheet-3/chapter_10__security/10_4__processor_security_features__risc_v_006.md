---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 10. Security
section: 10.4. Processor security features (RISC-V)
pages: 822-822
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 10.4. Processor security features (RISC-V)

RP2350 Datasheet

in general, to get the correct NSC attribute on the Secure Gateway region), this part of the ROM becomes non-

executable to Secure code. Consequently, this part of the bootrom is not ROP-hardened. This part of the ROM contains

the NSBOOT (including USB boot) implementation, as well as a RISC-V Armv6-M emulator that can be used to emulate

most of the bootrom on RISC-V processors. This region is only implemented on the instruction-side IDAU query: this is

an implementation detail that improves timing on the load/store IDAU query, and does not have security implications

(given the mask ROM is inherently unwritable) other than that the tt instruction will not be aware of this region.

The final 512 bytes of the bootrom has the Secure, Non-secure-Callable (NSC) attribute. This means it contains entry

points for Non-secure calls into Secure code. Note that for this IDAU-defined attribute to take effect, the SAU-defined

attribute for this range must also be NSC or lower. The recommended configuration is a single Non-secure SAU region

covering the entirety of the bootrom. The bootrom exits into user code with the SAU enabled, and SAU region 7 active

and covering the entirety of the bootrom.

XIP and SRAM are Non-secure in the IDAU, as they are expected to be divided using the SAU. When the SAU and IDAU

differ, if the IDAU attribute is not Exempt, the processor takes whichever is greater out of the SAU and IDAU attribute, in

the order Secure > Non-secure-Callable > Non-secure.

Addresses not listed in this table are not decoded by the system AHB crossbar, and will return bus faults if accessed. In

these ranges, the ROM’s IDAU map is mirrored every 32 kB up to 0x0fffffff. The remaining addresses in the IDAU are

Non-secure.

10.3. Overview (RISC-V)

The RP2350 bootrom does not implement secure boot for RISC-V processors. Secure flash boot can still be

implemented on RISC-V by storing secure boot code in OTP and disabling other boot media via the BOOT_FLAGS0 row

in OTP. However, this is not supported natively by the RP2350 bootrom.

The RISC-V processors on RP2350 implement Machine and User execution modes, and the standard Physical Memory

Protection unit (PMP), which can be used to enforce internal security or safety boundaries. See Section 10.4.

Non-processor-specific hardware security features, such as debug disable OTP flags and the glitch detectors, are

functionally identically between Arm and RISC-V. However, the redundancy coprocessor (RCP) is not accessible from

the RISC-V processors, as it uses a Cortex-M33-specific coprocessor interface.

10.4. Processor security features (RISC-V)

The Hazard3 processors on RP2350 implement the following standard RISC-V security features:

• Machine and User execution modes (M-mode and U-mode)
• The Physical Memory Protection unit (PMP)

M-mode has full access to the processor’s internal status registers, but U-mode does not. The processor’s bus

accesses are tagged with its current execution mode and filtered by ACCESSCTRL bus filters, as described in Section

10.6.2.

The processor starts in M-mode, and enters M-mode upon taking any trap (exception or interrupt). It enters U-mode only

by executing a return-from-M-mode instruction, mret, with previous privilege set to U-mode. This means all interrupts

initially target M-mode, but can be de-privileged to U-mode via software routing. Because stacks are software-managed

on RISC-V, software cooperation is required to fully separate the two execution contexts, though there are enough

hardware hooks to make this possible. For more details about interrupts and exceptions on RISC-V, and how they relate

to the core’s privilege levels, see Section 3.8.4.

The PMP is a memory protection unit built into each RISC-V processor that filters every instruction execution address

and every load/store address against a list of permission regions. The Hazard3 instances on RP2350 are configured

with 8 PMP regions each, with a 32-byte granule and naturally-aligned power-of-2 region support only.

Additionally, there are 3 PMP-hardwired regions, which set a default User-mode RW permission on peripherals and a

10.3. Overview (RISC-V)
821
