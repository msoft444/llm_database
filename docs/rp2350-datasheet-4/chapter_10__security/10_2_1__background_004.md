---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 10. Security
section: 10.2.1. Background
pages: 820-820
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 10.2.1. Background

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

10.2. Processor security features (Arm)
819
