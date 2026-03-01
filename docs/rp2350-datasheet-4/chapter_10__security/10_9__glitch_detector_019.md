---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 10. Security
section: 10.9. Glitch detector
pages: 870-870
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 10.9. Glitch detector

10.9. Glitch detector

The glitch detector detects loss of setup and hold margin in the system clock domain, which may be caused by

deliberate external manipulation of the system clock or core supply voltage. When it detects loss, the glitch detector

triggers a system reset rather than allowing software to continue to execute in a possibly undefined state. It responds

10.8. OTP
869
