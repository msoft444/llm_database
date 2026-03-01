---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 10. Security
section: 10.6.1. GPIO access control
pages: 824-824
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 10.6.1. GPIO access control

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
