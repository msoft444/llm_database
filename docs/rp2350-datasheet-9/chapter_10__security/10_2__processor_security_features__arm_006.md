---
source_pdf: rp2350-datasheet-9.pdf
repository: llm_database
chapter: Chapter 10. Security
section: 10.2. Processor security features (Arm)
pages: 820-820
type: technical_spec
generated_at: 2026-03-01T11:44:31.548839+00:00
---

# 10.2. Processor security features (Arm)

10.2. Processor security features (Arm)

The Cortex-M33 processors on RP2350 are configured with the following standard Arm security features:

• Support for the Armv8-M Security extension
• 8× security attribution unit (SAU) regions
• 8× Secure and 8× Non-secure memory protection unit (MPU) regions

These features are covered exhaustively in the Armv8-M Architecture Reference Manual, the Cortex-M33 Technical

Reference Manual, and the Cortex-M33 section of this datasheet (Section 3.7). This section gives a high-level overview

of these features, as well as a description of the implementation-defined attribution unit included in RP2350.
