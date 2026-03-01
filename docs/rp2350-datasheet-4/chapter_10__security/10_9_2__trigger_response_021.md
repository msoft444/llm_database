---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 10. Security
section: 10.9.2. Trigger response
pages: 871-871
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 10.9.2. Trigger response

10.9.2. Trigger response

When any of the detectors fires, the corresponding bit in the TRIG_STATUS is set. If the glitch detector block is armed,

this detector event also resets almost all logic in the switched core domain. The glitch detector is armed if:

• The DISARM register is not set to the disarming bit pattern, and at least one of the following is true:

◦The GLITCH_DETECTOR_EN OTP flag was programmed some time before the most recent reset of the OTP block

◦The ARM register is set to an arming bit pattern

This holds the majority of the switched core domain in reset for approximately 120 microseconds before releasing the

reset. Specifically, this resets the PSM (Section 7.3), which resets all PSM-controlled resets starting with the processor

cold reset domain, in addition to all blocks reset by the RESETS block, which is itself reset by the PSM. The detector

circuits are also reset, as is the system watchdog including the watchdog scratch registers.

After a glitch detector-initiated reset, the CHIP_RESET.HAD_GLITCH_DETECT flag is set so that software can diagnose

that the last reset was caused by a glitch detector trigger. Check the TRIG_STATUS register to see which detector fired.

This can be useful for tuning the thresholds of individual detectors.

The only way to clear the detector circuits is to reset them, either via a full switched core domain reset (such as the RUN

pin, the SW-DP reset request, a PoR/BoR reset, or a reset of the switched core domain configured by POWMAN

controls), or by arming the glitch detector block so that the detectors reset along with the PSM.

Recovering from the glitch detector firing requires the low-power oscillator to be running (Section 8.4). Allowing the

10.9. Glitch detector
870

## Embedded Images

![img_p0871_00.png](images/img_p0871_00.png)

