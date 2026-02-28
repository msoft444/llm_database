---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 14. Electrical and mechanical
section: 14.6. Solder profile
pages: 1332-1333
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 14.6. Solder profile

◦See Section 12.14 for more details of the RP2350 QSPI memory interface and its capabilities
The internal flash die can also be programmed externally by holding the RP2350 die in reset via the RUN pin (active-low
reset), and driving QSPI signals into the chip from an external programmer.
The internal flash is powered by the QSPI_IOVDD supply input. This voltage must be in the range 2.7 to 3.6 V. You
should account for the increased high-frequency currents on this supply pin in your decoupling circuit and PCB layout.
The maximum QSPI clock frequency of the W25Q16JVWI is 133 MHz. Consult the W25Q16JVWI datasheet for detailed
timings and AC parameters.
If you do not require access to the RP2350 QSPI bus from the outside, you should minimise the track length connected
to the QSPI package pins on your PCB. This avoids unnecessary emissions and capacitive loading of the QSPI bus.
The PADRESETB reset input on the W25Q16JVWI is not connected to any external package pins, or to any internal
signals on the RP2350 die. This means there is no way to perform a hardware reset of the flash die. When the RP2350
die comes out of reset it initialises the flash die in the same way it would an external flash device by issuing a fixed XIP
exit sequence that returns the flash die to a serial command state in preparation for execute-in-place setup.
14.4. Package markings
RP2350 comes in 7 × 7 mm QFN-60 and 10 × 10 mm QFN-80 packages, which are marked with the following data:
• Pin 1 dot
• Logo
• Part number
• Date code (Week)
• Silicon lot number
• Date code (Year)
The part number consists of the following:
• Device name "RP2350"
• Package type, "A" for QFN-60 or "B" for QFN-80
• Package revision "0"
• Die stepping "A2", "A3", or "A4"
See Appendix C for a summary of the differences between die steppings.
14.5. Storage conditions
To preserve the shelf and floor life of bare RP2350 devices, follow JEDEC J-STD (020E & 033D).
RP2350 QFN-60 is classified as Moisture Sensitivity Level 1 (MSL1). The MSL of QFN-80 is still being characterised and
details will follow in a future datasheet update.
All RP2350 devices should be stored under 30°C and 85% relative humidity.
14.6. Solder profile
RP2350 is a Pb-free part, with a Tp value of 260°C.
All temperatures refer to the centre of the package, measured on the package body surface that faces up during
RP2350 Datasheet
14.4. Package markings
1331


assembly reflow (live-bug orientation). If parts are reflowed in a different orientation (e.g. dead-bug), Tp shall be within
±2°C of the live-bug Tp and still meet the Tc requirements; otherwise, you must adjust the profile to achieve the latter.
Time
Temperature
Tp
TL
Tc -5°C
Tsmax
Preheat Area
25
Time 25°C to Peak
Tsmin
ts
tp
t
Max. Ramp Up Rate = 3°C/s
Max. Ramp Down Rate = 6°C/s
User tp
User Tp ≤ Tc
Supplier t p
Supplier T p ≥ Tc
Tc -5°C
Tc
Figure 147.
Classification profile
(not to scale)
Reflow profiles in this document are for classification/preconditioning, and are not meant to specify board assembly
profiles. Actual board assembly profiles should be developed based on specific process needs and board designs, and
should not exceed the parameters in Table 1425.
Table 1425. Solder
profile values
Profile feature
Value
Temperature min (Tsmin)
150°C
Temperature max (Tsmax)
200°C
Time (ts) from (Tsmin to Tsmax)
60 - 120 seconds
Ramp-up rate (TL to Tp)
3°C/second max.
Liquidous temperature (TL)
217°C
Time (tL) maintained above TL
60 to 150 seconds
Peak package body temperature (Tp)
260°C
Classification temperature (Tc)
260°C
Time (tp) within 5°C of the specified classification temperature (Tc)
30 seconds
Ramp-down rate (Tp to TL)
6°C/second max.
Time 25°C to peak temperature
8 minutes max.
RP2350 Datasheet
14.6. Solder profile
1332

