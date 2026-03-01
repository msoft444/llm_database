---
source_pdf: rp2350-datasheet-6.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.1.1. Secure and Non-secure SIO
pages: 38-39
type: technical_spec
generated_at: 2026-03-01T05:39:21.783275+00:00
---

# 3.1.1. Secure and Non-secure SIO

![Page 38 figure](images/fig_p0038.png)

3.1.1. Secure and Non-secure SIO

To allow isolation of Secure and Non-secure software, whilst keeping a consistent programming model for software

written to run in either domain, the SIO is duplicated into a Secure and a Non-secure bank. Most hardware is duplicated

between the two banks, including:

• Mailbox FIFOs
• Doorbell registers

3.1. SIO
37

RP2350 Datasheet

• Interrupt outputs to processors
• Spinlocks

For example, Non-secure code on core 0 can pass messages to Non-secure code on core 1 through the Non-secure

instance of the mailbox FIFO. In turn, this message will generate a Non-secure interrupt, which is separate from the

Secure FIFO interrupt line. This does not interfere with any Secure message passing that might be going on at the same

time, and Non-secure code can not snoop Secure messages because it does not have access to the Secure mailboxes.

The software running in the Secure and Non-secure domain can be identical, and the processors' bus accesses to the

SIO will automatically be routed to the Secure or Non-secure version of the mailbox registers.

The following hardware is not duplicated:

• The GPIO registers are shared, and Non-secure accesses are filtered on a per-GPIO basis by the Non-secure GPIO

mask defined in the ACCESSCTRL GPIO_NSMASK0 and GPIO_NSMASK1 registers
• The RISC-V standard platform timer (MTIME, MTIMEH), which is also usable by Arm processors, is present only in

the Secure SIO, as it is a Machine-mode peripheral on RISC-V
• The interpolator and TMDS encoder peripherals are assignable to either the Secure or Non-secure SIO using the

PERI_NONSEC register

Accesses to the SIO register address range, starting at 0xd0000000 (SIO_BASE), are mapped to the SIO bank which

matches the security attribute of the bus access. This means accesses from the Arm Secure state, or RISC-V Machine

mode, will access the Secure SIO bank, and accesses from the Arm Non-secure state, or RISC-V User mode, will access

the Non-secure SIO bank.

Additionally, Secure accesses can use the mirrored address range starting at 0xd0020000 (SIO_NONSEC_BASE) to access

the Non-secure view of SIO, for example, using the Non-secure doorbells to interrupt Non-secure code running on the

other core. Attempting to access this address range from Non-secure code will generate a bus fault.

NOTE

The 0x20000 offset of the Secure-to-Non-secure mirror matches the PPB mirrors at 0xe0000000 (PPB_BASE) and

0xe0020000 (PPB_NONSEC_BASE), which function similarly.

NOTE

Debug access is mapped to the Secure/Non-secure SIO using the security attribute of the debugger’s bus access,

which may differ from the security state that the core was halted in.
