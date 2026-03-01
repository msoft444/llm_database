---
source_pdf: rp2350-datasheet-8.pdf
repository: llm_database
chapter: Chapter 13. OTP
section: 13.3.4. Power-up state machine
pages: 1273-1274
type: technical_spec
generated_at: 2026-03-01T08:45:43.944040+00:00
---

# 13.3.4. Power-up state machine

13.3.4. Power-up state machine

The OTP is the second item in the switched core domain’s Power-On State Machine (Section 7.4), after the processor

cold reset. OTP does not release its rst_done, or enable any debug interface (including the factory test JTAG described in

Section 10.10), until the OTP PSM reads out OTP-resident hardware configuration. The rst_done output to the system

13.3. Background: OTP hardware architecture
1272

RP2350 Datasheet

PSM holds the rest of the system in reset until the OTP PSM completes, so that no software runs until the OTP’s

contents are known.

The OTP boot sequence runs from a local ring oscillator. This oscillator is dedicated to the OTP subsystem, and is

separate from the main system ROSC used by the processors at boot. The sequence is:

1. First, the PSM runs the Synopsys boot instruction. This has the following steps:

a. Wait for the power supply to return a 'good' value.

b. Read consistency check location until hardware sees the correct value for 16 successive reads. Consistency

checks use predefined words stored in mask ROM cells with similar analogue properties to OTP cells.

2. Read critical flags (non-ECC): each critical bit is redundant across 8 OTP rows, with three-of-eight vote for each

flag.

3. Read hardware access keys via ECC read interface.

4. Read valid bits for hardware access keys, including the debug keys (Section 3.5.9.2)

5. Initialise page lock registers from the lock page via raw read interface.

6. Assert rst_done signal to the system power-on state machine

7. The system reset sequence continues, starting with the system ROSC

RP2350 A3 adds correctness checks and robustness to the PSM. For more information about these additions, see

RP2350-E16.
