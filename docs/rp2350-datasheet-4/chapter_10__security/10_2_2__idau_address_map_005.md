---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 10. Security
section: 10.2.2. IDAU address map
pages: 821-821
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 10.2.2. IDAU address map

10.2.2. IDAU address map

The Cortex-M33 provides an implementation-defined attribution unit (IDAU) interface, which allows system

implementers such as Raspberry Pi Ltd to augment the security attribution map defined by the SAU. The RP2350 IDAU

is a hardwired address decode network, with no user configuration. Its address map is as follows:

| Start (hex) | End (hex) | Contents | IDAU Attribute |
| --- | --- | --- | --- |
| 00000000 | 000042ff | Arm boot | Exempt |
| 00004300 | 00007dff | USB/RISC-V boot | Non-secure (instruction fetch), Exempt (load/store) |
| 00007e00 | 00007fff | Bootrom SGs | Secure and Non-secure-Callable |
| 10000000 | 1fffffff | XIP | Non-secure |
| 20000000 | 20081fff | SRAM | Non-secure |
| 40000000 | 4fffffff | APB | Exempt |
| 50000000 | 5fffffff | AHB | Exempt |
| d0000000 | dfffffff | SIO | Exempt |

Exempt regions are not checked by the processor against its current security state. Effectively, the processor considers

these regions Secure when the processor is in the Secure state, and Non-secure when the processor is in the Non-

secure state.

Peripherals are marked Exempt because you’re expected to assign them to security domains using the controls in

ACCESSCTRL (Section 10.6). This approach avoids relying on SAU regions, which are too limited for meaningful

peripheral assignment, and eliminates the need for separate Secure and Non-secure peripheral mirrors, which can

cause programming errors.

The SIO is marked Exempt because it is internally banked over Secure and Non-secure based on the bus access’s

security attribute, which generally matches the processor’s current security state.

As peripherals are Exempt, RP2350 forbids processor instruction fetch from peripherals, by physically disconnecting the

bus. Processors fail to fetch instructions from peripherals even if the default MPU permissions are overridden to allow

execute permission. Exempt regions permit both Secure and Non-secure access, and TrustZone-M forbids the

combination of Non-secure-writable and Secure-executable, so this is a necessary restriction. The same consideration

does not apply to the bootrom as the ROM is physically immutable.

The first part of the bootrom is Exempt, because it contains routines expected to be called by both Secure and Non-

secure software in cases where it may not be desirable for Non-secure code to elevate through a Secure Gateway. An

example of this is the bootrom memcpy() implementation. Code in the Exempt ROM region is hardened against return-

oriented programming (ROP) attacks using the redundancy coprocessor’s stack canary instructions.

After a certain watermark, which may vary depending on ROM revision, the ROM becomes IDAU-Non-secure for the

purpose of instruction fetch. If an Non-secure SAU region is placed over the bootrom (which is expected to be the case

10.2. Processor security features (Arm)
820
