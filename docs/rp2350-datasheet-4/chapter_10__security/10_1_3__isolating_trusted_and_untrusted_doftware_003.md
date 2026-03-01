---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 10. Security
section: 10.1.3. Isolating trusted and untrusted doftware
pages: 819-819
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 10.1.3. Isolating trusted and untrusted doftware

10.1.3. Isolating trusted and untrusted doftware

In security- or safety-critical applications, access must be limited to those who need it. For example, a JPEG decode

library should not be able to access the core voltage regulator and increase DVDD to 3.3 V (unless you are

decompressing a very large JPEG). The Cortex-M33 processors contain hardware that separates two execution

contexts, known as Secure and Non-secure, and enforces a number of invariants between them, such as:

• Non-secure code cannot access Secure memory
• The Secure context cannot execute Non-secure memory
• Non-secure code cannot directly access peripherals managed by Secure code
• Non-secure code cannot prevent Secure interrupts from being serviced

By making less of your code able to access your most critical hardware and data, you reduce the chance of accidentally

exposing this critical hardware and data to the outside world. For a high-level explanation of how the Cortex-M33

implements this, see Section 10.2. For full details, see Armv8-M Architecture Reference Manual.

10.1. Overview (Arm)
818
