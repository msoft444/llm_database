---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 8. Clocks
section: 8.3.7. ROSC divider
pages: 564-564
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 8.3.7. ROSC divider

8.3.5. Modifying the frequency
The ROSC is arranged as 8 stages, each with programmable drive. The ROSC provides two methods of controlling the
frequency. The frequency range controls the number of stages in the ROSC loop and the FREQA & FREQB registers control
the drive strength of the stages.
To change the frequency range, write to the FREQ_RANGE register, which controls the number of stages in the ROSC loop.
The FREQ_RANGE register supports the following configurations:
Table 604. ROSC
stage ranges
Name
Number of stages
Range (stages)
LOW
8
0-7
MEDIUM
6
2-7
HIGH
4
4-7
TOOHIGH
2
6-7
Change FREQ_RANGE one step at a time until you reach the desired range. When increasing the frequency range, ROSC
output will not glitch, so the output clock can continue to be used. When decreasing the frequency range, ROSC output
will glitch, so you must select an alternate clock source for the modules clocked by ROSC or hold them in reset during
the transition.
The behaviour has not been fully characterised, but the MEDIUM range will be approximately 1.33 times the LOW range, the
HIGH range will be 2 times the LOW range and the TOOHIGH range will be 4 times the LOW range. The TOOHIGH range is aptly
named. It should not be used because the internal logic of the ROSC will not run at that frequency.
The FREQA and FREQB registers control the drive strength of the stages in the ROSC loop. As the drive strength increases,
the delay through the stage decreases and the oscillation frequency increases. Each stage has 3 drive strength control
bits. Each bit turns on an additional drive, therefore each stage has 4 drive strength settings equal to the number of bits
set, with 0 being the default, 1 being double drive, 2 being triple drive and 3 being quadruple drive. Extra drives do not
have a linear effect on frequency: the second has less impact than the first, the third has less impact than the second,
and so on. To ensure smooth transitions, change one drive strength bit at a time. When FREQ_RANGE shortens the ROSC
loop, the bypassed stages still propagate the signal and therefore their drive strengths must be set to at least the same
level as the lowest drive strength in the stages that are in the loop. This will not affect the oscillation frequency.
8.3.6. Randomising the frequency
Randomisation is enabled by setting the drive strength controls for the first two stages of the ROSC loop to DS0_RANDOM
and DS1_RANDOM. An LFSR then provides the drive strength controls for those two stages which are always included in the
loop regardless of the FREQ_RANGE setting. It is recommended to randomise both stages. When the low FREQ_RANGE is
selected the randomiser will increase the frequency by up to 22% of the default. The increase will be approximately half
of that if only one stage is randomised. The LFSR can be seeded by writing to the RANDOM register. This can be done at
any time but will restart the randomiser.
8.3.7. ROSC divider
The ROSC frequency is too fast to be used directly, so it is divided in an integer divider controlled by the DIV register. You
can change DIV while the ROSC is running, and the output clock will change frequency without glitching. The default
divisor is 8, which ensures the output clock is in the specified range on chip startup.
The divider has two outputs, rosc_clksrc and rosc_clksrc_ph. rosc_clksrc_ph is a phase shifted version of rosc_clksrc. This
is primarily intended for use during product development; the outputs are identical if the PHASE register is left in its
default state.
RP2350 Datasheet
8.3. Ring oscillator (ROSC)
563

