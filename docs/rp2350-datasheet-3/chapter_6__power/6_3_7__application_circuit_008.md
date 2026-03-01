---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 6. Power
section: 6.3.7. Application circuit
pages: 451-452
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 6.3.7. Application circuit

RP2350 Datasheet

CAUTION

Low-power mode should only be used when the regulator is providing the chip’s digital core supply (DVDD) because

the regulator’s low-power output is connected to DVDD on chip.

6.3.4. Status

To determine the status of the regulator, read the VREG_STS register, which contains two fields:

• VOUT_OK indicates whether the voltage regulator’s output is being correctly regulated. At power-on, VOUT_OK remains

low until the regulator has started up and the output voltage reaches the VOUT_OK assertion threshold (VOUT_OKTH.ASSERT).

It then remains high until the voltage drops below the VOUT_OK de-assertion threshold (VOUT_OKTH.DEASSERT), remaining low

until the output voltage is above the assertion threshold again. VOUT_OKTH.ASSERT is nominally 90% of the selected output

voltage, 0.99 V if the selected output voltage is 1.1 V, and VOUT_OKTH.DEASSERT is nominally 87% of the selected output

voltage, 0.957 V if the selected output voltage is 1.1 V. See Section 14.9.6 for details.
• STARTUP is high when the regulator is starting up, and remains high until the regulator’s operating mode or output

voltage are changed, either by software or the Power Manager

Adjusting the output voltage to a higher voltage will cause VOUT_OK to go low until the assertion threshold for the higher

voltage is reached. VOUT_OK will also go low if the regulator is placed in high-impedance mode.

6.3.5. Current limit

The voltage regulator includes a current limit to prevent the load current exceeding the maximum rated value. The

output voltage won’t be regulated and will drop below the selected value when the current limit is active. See Section

14.9.6 for details.

6.3.6. Over temperature protection

The voltage regulator will terminate regulation and disable its power transistors, if the transistor junction temperature

rises above a threshold set by the HT_TH field in the VREG_CTRL register. The regulator will restart regulation when the

transistor junction temperature drops to approximately 20°C below the temperature threshold.

6.3.7. Application circuit

The regulator requires two external power supplies, the input supply (VREG_VIN), and a separate low noise supply for its

analogue control circuits (VREG_AVDD). VREG_VIN must be in the range 2.7 V to 5.5 V, and VREG_AVDD must be in the range

3.135 V to 3.63 V.

If VREG_VIN is limited to the range 3.135 V to 3.63 V, a single combined supply can be used for both VREG_VIN and

VREG_AVDD. This approach is shown in Figure 19. Take care to minimise noise on VREG_AVDD.

6.3. Core voltage regulator
450

![Page 452 figure](images/fig_p0452.png)

RP2350 Datasheet

Figure 19. Core

voltage regulator with

3.135V to 3.63V supply

combined supplies

4.7µF

4.7µF
3.3µH

33Ω

GND

4.7µF

VREG_PGND

VREG_FB

VREG_LX

VREG_AVDD

VREG_VIN

GND

DVDD
DVDD

100nF

100nF

GND

GND

DVDD

4.7µF

GND

Alternatively, to support input voltages above 3.63 V, VREG_VIN and VREG_AVDD can be powered separately. This is shown in

Figure 20.

Figure 20. Core

voltage regulator with

2.7V to 5.5V supply

separate supplies

4.7µF

4.7µF
3.3µH

GND

3.135V to 3.63V supply

VREG_PGND

VREG_FB

VREG_LX

VREG_AVDD

VREG_VIN

DVDD
DVDD

100nF

100nF

GND

GND

DVDD

4.7µF

GND

If the digital core supply (DVDD) is powered from an external 1.1V supply, the on-chip regulator can be disabled and the

application circuit simplified. Power must still be provided on the regulator’s analogue supply (VREG_AVDD) and input

supply (VREG_VIN) to power the chip’s power-on reset and brown-out detection blocks. But the inductor can be omitted

and only a single input capacitor is required. Connect VREG_FB directly to ground. This is shown in Figure 21.

6.3. Core voltage regulator
451
