---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 10. Security
section: 10.2.1. Background
pages: 820-820
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 10.2.1. Background

To make the programming model of Secure and Non-secure software consistent, and to avoid overhead in Non-secure
code, RP2350 extends Secure/Non-secure separation throughout the system. For example, DMA channels can be
assigned for Secure or Non-secure use. Using this extended separation, Non-secure code can use DMA transfers to
accelerate peripheral accesses without endangering security model invariants (such as Non-secure code using the DMA
to read Secure memory).
The key hardware features that enable Secure/Non-secure separation throughout the system are:
• The Cortex-M33’s implementation of the Secure and Non-secure states (Section 10.2)
• The DMA’s implementation of matching per-channel security states (Section 10.7)
• The system-level bus access filtering implemented by ACCESSCTRL (Section 10.6)
• Peripheral-level filtering, such as the per-GPIO access filtering of the SIO GPIO registers (Section 3.1.1)
10.2. Processor security features (Arm)
The Cortex-M33 processors on RP2350 are configured with the following standard Arm security features:
• Support for the Armv8-M Security extension
• 8× security attribution unit (SAU) regions
• 8× Secure and 8× Non-secure memory protection unit (MPU) regions
These features are covered exhaustively in the Armv8-M Architecture Reference Manual, the Cortex-M33 Technical
Reference Manual, and the Cortex-M33 section of this datasheet (Section 3.7). This section gives a high-level overview
of these features, as well as a description of the implementation-defined attribution unit included in RP2350.
10.2.1. Background
The Cortex-M33 processors on RP2350 support the Armv8-M Security Extension. Hardware in the processor maintains
two separate execution contexts, called the Secure and Non-secure domains. Access to important data, such as
cryptographic keys, or hardware, such as the system voltage regulator, can be limited to the Secure domain. Separating
execution into these domains prevents Non-secure execution from interfering with Secure execution. When this
datasheet uses the (capitalised) terms Secure and Non-secure, we refer to these two Arm security domains and the
associated bus attributes.
Code running in the Non-secure domain is not necessarily malicious. Consider complex protocols and stacks like USB,
whose implementation is expected to be easily-exploited and prone to fatal crashes. Restricting such software to the
Non-secure domain helps isolate critical software from the consequences of those design decisions. The RP2350
bootrom, for example, runs all of its USB code in the Non-secure domain so the USB code does not have to be
considered in the design of critical parts of the bootrom, such as boot signature enforcement.
At any given moment, an Armv8-M processor implementing the Security Extension is in either the Secure execution
state or the Non-secure execution state. Based on the current state, the processor limits the executable memory
regions and the memory regions accessible via load/store instructions. All of the processor’s AHB accesses are tagged
according to the state that originated them, so that peripherals and the system bus fabric itself can filter transfers
based on security domain, for example, using the access control lists described in Section 10.6.
An internal processor peripheral called the Security Attribution Unit (SAU) defines, from the processor’s point of view,
which address ranges are accessible to the Secure and Non-secure domains. The number of distinct address ranges
which can be decoded by the SAU is limited, which is why system-level bus filters are provided for assigning peripherals
to security domains.
The processor changes security state synchronously using special function calls between states. When an interrupt
routed to the Secure domain occurs, the processor can also change security state asynchronously if in the Non-secure
state, or vice versa (if enabled).
Both Cortex-M33 processors on RP2350 implement the security extension, so each processor maintains its own Secure
RP2350 Datasheet
10.2. Processor security features (Arm)
819

