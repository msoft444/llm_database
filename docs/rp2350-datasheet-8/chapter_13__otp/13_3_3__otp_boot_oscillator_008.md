---
source_pdf: rp2350-datasheet-8.pdf
repository: llm_database
chapter: Chapter 13. OTP
section: 13.3.3. OTP boot oscillator
pages: 1273-1273
type: technical_spec
generated_at: 2026-03-01T08:45:43.944040+00:00
---

# 13.3.3. OTP boot oscillator

13.3.3. OTP boot oscillator

The OTP startup sequence (Section 13.3.4) runs from a local ring oscillator, dedicated to the OTP subsystem. This is

separate from the system ring oscillator (the ROSC) which provides the system clock to run the processors during boot.

• The OTP boot oscillator is the only clock used by the OTP power-up state machine
• The OTP boot oscillator dynamically randomises its own frequency controls, to deliberately add jitter to the clock
• The OTP boot oscillator stops when the PSM completes, and does not start again until the OTP resets
• The OTP clock automatically switches to clk_ref when the OTP boot oscillator stops

The boot oscillator has a nominal frequency of 12MHz. It provides the clock for reading out hardware configuration

from OTP, including the critical flags (Section 13.4) which configure hardware security features such as debug disable

and the glitch detectors.

Keeping this oscillator local to the OTP hardware subsystem reduces the power signature of the clock itself, due to the

lower switched clock capacitance. Along with the random jitter of the frequency controls, this helps frustrate attempts

to recover OTP access keys and debug keys via power signature analysis attacks, or to disable security features by

timing fault injection against the OTP clock.

Only the OTP boot oscillator enables the ROSC frequency randomisation feature by default: for later operations using

the system ROSC (Section 8.3), you must explicitly enable this feature on that oscillator, by programming the ROSC

control registers. The crystal oscillator (XOSC) does not support frequency randomisation.
