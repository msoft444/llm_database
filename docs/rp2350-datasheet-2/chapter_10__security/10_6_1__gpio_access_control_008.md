---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 10. Security
section: 10.6.1. GPIO access control
pages: 824-824
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 10.6.1. GPIO access control

RP2350 Datasheet

• Non-secure code access to that GPIO’s IO muxing and pad control registers
• GPIO selection access to peripherals accessible only via Secure bus access

ACCESSCTRL registers are always fully readable by the processors in any security or privilege state, so that Non-secure

software can enumerate the hardware it is allowed to access. However, writes to ACCESSCTRL are strictly controlled.

Unprivileged writes, and writes from the DMA, return a bus fault. Writes from a Non-secure, Privileged (NSP) context are

generally ignored, with the sole exception of the Non-secure, Unprivileged (NSU) bits in bus access control registers. The

NSU bits are Non-secure-writable if and only if the NSP bit is set.

Writes can be further locked down using the LOCK register. This causes writes from specific managers to be ignored.

For a full list of effects, see Section 10.6.1.

To reduce the risk of accidental writes, all ACCESSCTRL registers, except GPIO_NSMASK0 and GPIO_NSMASK1, require

the 16-bit value 0xacce to be present in the most-significant 16 bits of the write data. To achieve this, OR the value

0xacce0000 with your write data. Atomic SET/CLR/XOR alias writes must also include this value. DMA writes are also

forbidden, to avoid accidentally wiping permissions with a misconfigured DMA channel.

IMPORTANT

Writes with the upper 16 bits not equal to 0xacce both fail and return a bus fault (instead of silently leaving the

permissions unchanged).

Finally, the FORCE_CORE_NS register makes core 1’s bus accesses appear to be Non-secure at system level. This

supports schemes where all Secure services run on core 0, and therefore core 1 should not be able to access Secure

hardware.

10.6.1. GPIO access control

The GPIO Non-secure access mask registers, GPIO_NSMASK0 and GPIO_NSMASK1, contain one bit per GPIO. The

layout of these two registers matches the layout of the SIO GPIO registers (Section 3.1.3), including the positions of the

QSPI and USB DM/DP bits. Each GPIO is accessible to Non-secure software if and only if the relevant GPIO_NSMASK bit is

set. This prevents Non-secure software from interfering with or observing GPIOs used by Secure software.

All system-level GPIO controls, such as the IO and pad control registers, are shared by Secure and Non-secure code.

However, access to these registers is filtered on a GPIO-by-GPIO basis according to the GPIO_NSMASK registers. This

means that the same code can run unmodified in either a Secure or Non-secure context, and Secure software does not

have to implement any interfaces for Non-secure GPIO access, provided that the appropriate GPIO security mask has

been configured.

Setting a GPIO_NSMASK bit has the following effects on the corresponding GPIO:

• The relevant SIO GPIO register bit (Section 3.1.3) becomes accessible through bus access to the Non-secure SIO.

◦Otherwise the bit is read-only zero.
• The relevant SIO GPIO register bit becomes accessible to Non-secure code using GPIO coprocessor instructions

(Section 3.6.1).

◦Otherwise the bit is read-only zero.

◦Non-secure code may execute GPIO coprocessor instructions if and only if coprocessor 0 is granted to Non-

secure in NSACR, and enabled in the Non-secure PPB instance of the CPACR register.
• The relevant IO control register (Section 9.11.1 or Section 9.11.2) becomes accessible to Non-secure code.

◦Otherwise it is read-only zero.
• GPIO functions for Secure-only peripherals can not be selected on this GPIO

◦Attempting to select such a peripheral will select the null function (0x1f) instead

10.6. Access control
823
