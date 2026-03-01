---
source_pdf: rp2350-datasheet-6.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.1.1. Secure and Non-secure
pages: 355-355
type: technical_spec
generated_at: 2026-03-01T05:39:21.783275+00:00
---

# 5.1.1. Secure and Non-secure

5.1.1. Secure and Non-secure

This datasheet uses the (capitalised) terms Secure and Non-secure to refer to the Arm execution states of the same

name, defined in the Armv8-M Architecture Reference Manual. The uncapitalised term "secure" has no special meaning.

In some contexts, Secure can also refer to a RISC-V core, usually one running at the Machine privilege level. For

example, the low-level flash APIs are exported to Arm Secure code and RISC-V code only, so Secure serves as a

shorthand for this type of API.

A secured RP2350 is a device where secure boot is enabled (Section 5.10.1). This is not the same as the Secure state,

since the device may run a mixture of Secure and Non-secure code after completing the secure boot process.
