---
source_pdf: rp2350-datasheet-6.pdf
repository: llm_database
chapter: Chapter 10. Security
section: 10.9. Glitch detector
pages: 870-871
type: technical_spec
generated_at: 2026-03-01T05:39:21.783275+00:00
---

# 10.9. Glitch detector

10.9. Glitch detector

The glitch detector detects loss of setup and hold margin in the system clock domain, which may be caused by

deliberate external manipulation of the system clock or core supply voltage. When it detects loss, the glitch detector

triggers a system reset rather than allowing software to continue to execute in a possibly undefined state. It responds

10.8. OTP
869

RP2350 Datasheet

within one system clock cycle, unlike the brownout detector, which has much more limited analog bandwidth.

The glitch detector is disabled by default, and can be armed by setting the GLITCH_DETECTOR_ENABLE flag in OTP. For

debugging purposes, you can also enable the glitch detector via the ARM register. This is not recommended in security-

sensitive applications, as the system is vulnerable until the point that software can enable the detectors.

## Embedded Images

![img_p0871_00.png](images/img_p0871_00.png)

