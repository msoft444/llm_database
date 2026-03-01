---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 10. Security
section: 10.9.3. List of registers
pages: 872-874
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 10.9.3. List of registers

10.9.3. List of registers

The glitch detector control registers start at an address of 0x40158000.

| Offset | Name | Info |
| --- | --- | --- |
| 0x00 | ARM | Forcibly arm the glitch detectors, if they are not already armed by OTP. When armed, any individual detector trigger will cause a restart of the switched core power domain’s power-on reset state machine. Glitch detector triggers are recorded accumulatively in TRIG_STATUS. If the system is reset by a glitch detector trigger, this is recorded in POWMAN_CHIP_RESET. This register is Secure read/write only. |
| 0x04 | DISARM |  |
| 0x08 | SENSITIVITY | Adjust the sensitivity of glitch detectors to values other than their OTP-provided defaults. This register is Secure read/write only. |
| 0x0c | LOCK |  |
| 0x10 | TRIG_STATUS | Set when a detector output triggers. Write-1-clear. (May immediately return high if the detector remains in a failed state. Detectors can only be cleared by a full reset of the switched core power domain.) This register is Secure read/write only. |
| 0x14 | TRIG_FORCE | Simulate the firing of one or more detectors. Writing ones to this register will set the matching bits in STATUS_TRIG. If the glitch detectors are currently armed, writing ones will also immediately reset the switched core power domain, and set the reset reason latches in POWMAN_CHIP_RESET to indicate a glitch detector resets. This register is Secure read/write only. |

Table 971. List of

GLITCH_DETECTOR

registers

GLITCH_DETECTOR: ARM Register

Offset: 0x00

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:16 | Reserved. | - | - |

Table 972. ARM

10.9. Glitch detector
871

RP2350 Datasheet

![Page 873 figure](images/fig_p0873.png)

Bits
Description
Type
Reset

15:0
Forcibly arm the glitch detectors, if they are not already armed by OTP. When

armed, any individual detector trigger will cause a restart of the switched core

power domain’s power-on reset state machine.

Glitch detector triggers are recorded accumulatively in TRIG_STATUS. If the

system is reset by a glitch detector trigger, this is recorded in

This register is Secure read/write only.

0x5bad → NO: Do not force the glitch detectors to be armed

0x0000 → YES: Force the glitch detectors to be armed. (Any value other than

GLITCH_DETECTOR: DISARM Register

Table 973. DISARM

Register
Bits
Description
Type
Reset

31:16
Reserved.
-
-

15:0
Forcibly disarm the glitch detectors, if they are armed by OTP. Ignored if ARM

This register is Secure read/write only.

0x0000 → NO: Do not disarm the glitch detectors. (Any value other than

0xdcaf → YES: Disarm the glitch detectors

GLITCH_DETECTOR: SENSITIVITY Register

Adjust the sensitivity of glitch detectors to values other than their OTP-provided defaults.

This register is Secure read/write only.

Table 974.

SENSITIVITY Register
Bits
Description
Type
Reset

31:24
DEFAULT
RW
0x00

0x00 → YES: Use the default sensitivity configured in OTP for all detectors.

(Any value other than DEFAULT_NO counts as YES)

0xde → NO: Do not use the default sensitivity configured in OTP. Instead use

23:16
Reserved.
-
-

15:14
DET3_INV: Must be the inverse of DET3, else the default value is used.
RW
0x0

13:12
DET2_INV: Must be the inverse of DET2, else the default value is used.
RW
0x0

10.9. Glitch detector
872

RP2350 Datasheet

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 11:10 | DET1_INV: Must be the inverse of DET1, else the default value is used. | RW | 0x0 |
| 9:8 | DET0_INV: Must be the inverse of DET0, else the default value is used. | RW | 0x0 |
| 7:6 | DET3: Set sensitivity for detector 3. Higher values are more sensitive. | RW | 0x0 |
| 5:4 | DET2: Set sensitivity for detector 2. Higher values are more sensitive. | RW | 0x0 |
| 3:2 | DET1: Set sensitivity for detector 1. Higher values are more sensitive. | RW | 0x0 |
| 1:0 | DET0: Set sensitivity for detector 0. Higher values are more sensitive. | RW | 0x0 |

GLITCH_DETECTOR: LOCK Register

Offset: 0x0c

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:8 | Reserved. | - | - |
| 7:0 | Write any nonzero value to disable writes to ARM, DISARM, SENSITIVITY and LOCK. This register is Secure read/write only. | RW | 0x00 |

Table 975. LOCK

GLITCH_DETECTOR: TRIG_STATUS Register

Offset: 0x10

Description

Set when a detector output triggers. Write-1-clear.

(May immediately return high if the detector remains in a failed state. Detectors can only be cleared by a full reset of the

switched core power domain.)

This register is Secure read/write only.

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:4 | Reserved. | - | - |
| 3 | DET3 | WC | 0x0 |
| 2 | DET2 | WC | 0x0 |
| 1 | DET1 | WC | 0x0 |
| 0 | DET0 | WC | 0x0 |

Table 976.

GLITCH_DETECTOR: TRIG_FORCE Register

Offset: 0x14

10.9. Glitch detector
873
