---
source_pdf: rp2350-datasheet-6.pdf
repository: llm_database
chapter: Chapter 10. Security
section: 10.6. Access control
pages: 823-824
type: technical_spec
generated_at: 2026-03-01T05:39:21.783275+00:00
---

# 10.6. Access control

10.6. Access control

The access control registers (ACCESSCTRL) define permissions required to access GPIOs and bus endpoints such as

peripherals and memory devices.

For each bus endpoint (for example, PIO0), a bus access control register such as PIO0 controls which AHB5 managers

can access it, and at which bus security levels. This register has further implications, such as access to the RESETS

controls for that block. For a full explanation of the bus access control registers, see Section 10.6.2.

For each GPIO, including the QSPI and USB DP/DM pins, a bit in the GPIO_NSMASK0 and GPIO_NSMASK1 register can

be set to make that GPIO accessible to both the Secure and Non-secure domains, or clear to make it Secure-only. This

has system-wide implications, controlling:

• GPIO visibility to the Non-secure SIO

10.5. Secure boot enable procedure
822

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
