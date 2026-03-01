---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 13. OTP
section: 13.3.4. Power-up state machine
pages: 1273-1273
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 13.3.4. Power-up state machine

13.3.4. Power-up state machine

The OTP is the second item in the switched core domain’s Power-On State Machine (Section 7.4), after the processor

cold reset. OTP does not release its rst_done, or enable any debug interface (including the factory test JTAG described in

Section 10.10), until the OTP PSM reads out OTP-resident hardware configuration. The rst_done output to the system

13.3. Background: OTP hardware architecture
1272
