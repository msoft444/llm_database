---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 6. Power
section: 6.1.3. Digital core supply (DVDD)
pages: 442-442
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 6.1.3. Digital core supply (DVDD)

![Page 442 figure](images/fig_p0442.png)

RP2350 Datasheet

Chapter 6. Power

6.1. Power supplies

RP2350 requires five separate power supplies. However, in most applications, several of these can be combined and

connected to a single power source. Typical applications only require a single 3.3 V supply. See Figure 19.

The power supplies and a number of potential power supply schemes are described in the following sections. Detailed

power supply parameters are provided in Section 14.9.5.

6.1.1. Digital IO supply (IOVDD)

IOVDD provides the IO supply for the chip’s GPIO, and should be powered at a nominal voltage between 1.8 V and 3.3 V.

The supply voltage sets the external signal level for the digital IO, and should be chosen based on the level required, see

Section 14.9 for details. All GPIOs share the same power supply and operate at the same signal level.

If the digital IO is powered at a nominal 1.8 V, the IO input thresholds should be adjusted by setting the

VOLTAGE_SELECT register to 1. VOLTAGE_SELECT is set to 0 by default, which results in input thresholds that are valid

for a nominal IO voltage between 2.5 V and 3.3 V. See Chapter 9 for details.

CAUTION

Powering the IOVDD at 1.8 V with input thresholds set for a 2.5 V to 3.3 V supply is a safe operating mode, but will

result in input thresholds that do not meet specification. Powering the IO at voltages greater than 1.8 V with input

thresholds set for a 1.8 V supply may result in damage to the chip.

6.1.2. QSPI IO supply (QSPI_IOVDD)

QSPI_IOVDD provides the IO supply for the chip’s QSPI interface, and should be powered at a nominal voltage between

1.8 V and 3.3 V. The supply voltage sets the external signal level for the QSPI interface, and should be chosen based on

the level required, see Section 14.9 for details. In most applications the QSPI interface will be connected to an external

flash device, which will determine the required signal level.

If the QSPI interface is powered at a nominal 1.8 V, the IO input thresholds should be adjusted by setting the

VOLTAGE_SELECT register to 1. VOLTAGE_SELECT is set to 0 by default, which results in input thresholds that are valid

for a nominal IO voltage between 2.5 V and 3.3 V. See Chapter 9 for details.

CAUTION

Powering the IOVDD at 1.8 V with input thresholds set for a 2.5 V to 3.3 V supply is a safe operating mode, but will

result in input thresholds that do not meet specification. Powering the IO at voltages greater than 1.8 V with input

thresholds set for a 1.8 V supply may result in damage to the chip.

6.1.3. Digital core supply (DVDD)

The chip’s core digital logic is powered by DVDD, which should be at a nominal 1.1 V. A dedicated on-chip core voltage

regulator allows DVDD to be generated from a 2.7 V to 5.5 V input supply. See Section 6.3 for details. Alternatively, DVDD

can be supplied directly from an off-chip power source.

If the on-chip core voltage regulator is used, the two DVDD pins closest to the regulator should be decoupled with a 100nF

capacitor close to the pins. The DVDD pin furthest from the regulator should be decoupled with a 4.7μF capacitor close to

6.1. Power supplies
441
