---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.5.2. Programmer’s model
pages: 1078-1086
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 12.5.2. Programmer’s model

![Page 1078 figure](images/fig_p1078.png)

RP2350 Datasheet

12.5.1. Overview

Pulse width modulation (PWM) smoothly varies the average voltage of a digital signal using controlled-width positive

pulses at regular intervals. The fraction of time spent high is known as the duty cycle. This may be used to approximate

an analogue output or control switchmode power electronics.

The RP2350 PWM block has 12 identical slices. Each slice can drive two PWM output signals, or measure the frequency

or duty cycle of an input signal. The two outputs on each slice have the same period, but independently varying duty

cycles, so this gives a total of 24 controllable PWM outputs in the QFN-80 package.

Figure 111. A single

PWM slice. A 16-bit

counter counts from 0

up to some

programmed value,

and then wraps to

zero, or counts back

down again,

depending on PWM

mode. The A and B

outputs transition high

and low based on the

current count value

and the

preprogrammed A and

B thresholds. The

Each PWM slice is equipped with the following:

counter advances

based on a number of

• 16-bit counter
• 8.4 fractional clock divider
• Two independent output channels, duty cycle from 0% to 100% inclusive
• Dual slope and trailing edge modulation
• Edge-sensitive input mode for frequency measurement
• Level-sensitive input mode for duty cycle measurement
• Configurable counter wrap value

events: it may be free-

running, or gated by

level or edge of an

input signal on the B

pin. A fractional

divider slows the

overall count rate for

finer control of output

frequency.

◦Wrap and level registers are double buffered and can be changed race-free while PWM is running
• Interrupt request and DMA request on counter wrap
• Phase can be precisely advanced or retarded while running (increments of one count)

Slices can be enabled or disabled simultaneously via a single global control register. Slices then run in lockstep, so that

more complex power circuitry can be switched by the outputs of multiple slices.

12.5.1.1. Changes from RP2040

• Increased the number of slices from 8 to 12, with the 4 additional slices available on GPIOs 32 through 47 in the

QFN-80 package.
• Added a second shared interrupt line (controlled by IRQ1_INTE), to aid use of PWM slices as simple repeating

timers.

12.5.2. Programmer’s model

All GPIO pins on RP2350 can be used for PWM:

12.5. PWM
1077

![Page 1079 figure](images/fig_p1079.png)

RP2350 Datasheet

| GPIO | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| PWM Channel | 0A | 0B | 1A | 1B | 2A | 2B | 3A | 3B | 4A | 4B | 5A | 5B | 6A | 6B | 7A | 7B |
| GPIO | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 |
| PWM Channel | 0A | 0B | 1A | 1B | 2A | 2B | 3A | 3B | 4A | 4B | 5A | 5B | 6A | 6B | 7A | 7B |
| GPIO | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 40 | 41 | 42 | 43 | 44 | 45 | 46 | 47 |
| PWM Channel | 8A | 8B | 9A | 9B | 10A | 10B | 11A | 11B | 8A | 8B | 9A | 9B | 10A | 10B | 11A | 11B |

Table 1130. Mapping

of PWM channels to

GPIO pins on RP2350.

This is also shown in

the main GPIO

function table, Table

646

• The first 16 PWM channels (8 × 2-channel slices) appear on GPIOs 0 through 15, in the order PWM0 A, PWM0 B, PWM1 A,

and so on.
• This pattern repeats for GPIOs 16 through 31. GPIO16 is PWM0 A, GPIO17 is PWM0 B, and so on up to PWM7 B on GPIO31.

GPIO30 and above are available only in the QFN-80 package.
• The remaining 8 PWM channels (4 × 2-channel slices) appear on GPIOs 32 through 39, and then repeat on GPIOs

40 through 47.
• If you select the same PWM output on two GPIO pins, the same signal appears on both.
• If you use B pin as an input and select it on multiple GPIO pins, the PWM slice sees the logical OR of those two

GPIO inputs.

NOTE

GPIOs 0 through 29 have the same channel assignment as RP2040 for pinout compatibility. This reduces the

maximum number of independent PWM outputs in the QFN-60 package option of RP2350, but you can still use

slices 8 through 11 for repeating timer interrupts in this package.

12.5.2.1. Pulse width modulation (PWM)

The PWM hardware continuously compares an input value to a free-running counter. This produces a toggling output;

the amount of time spent at the high output level corresponds to the input value. The fraction of time spent at the high

signal level is known as the duty cycle of the signal.

The counting period is controlled by the TOP register, with a maximum possible period of 65536 cycles, as the counter

and TOP are 16 bits in size. Use the CC register to configure input values.

12.5. PWM
1078

![Page 1080 figure](images/fig_p1080.png)

RP2350 Datasheet

Figure 112. The

Input (Count)

counter repeatedly

Count

counts from 0 to TOP,

TOP

forming a sawtooth

Counter compare level

shape. The counter is

Counter

continuously

compared with some

TOP/3

|  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- |
|  | T | Ou | 2
tput (Pulse) | T | 3 |
|  |  |  |  |  |  |

input value. When the

input value is higher

0

than the counter, the

output is driven high.

Otherwise, the output

is low. The output

IOVDD

period T is defined by

GPIO pulse output

the TOP value of the

counter, and how fast

the counter is

configured to count.

The average output

0

T
2T
3T
t

voltage, as a fraction

of the IO power

supply, is the input

This example shows the counting period and the A and B counter compare levels being configured on one of RP2350’s

value divided by the

counter period (TOP +

PWM slices.

1)

Pico Examples: https://github.com/raspberrypi/pico-examples/blob/master/pwm/hello_pwm/hello_pwm.c Lines 14 - 29

14     // Tell GPIO 0 and 1 they are allocated to the PWM

15     gpio_set_function(0, GPIO_FUNC_PWM);

16     gpio_set_function(1, GPIO_FUNC_PWM);

17 

18     // Find out which PWM slice is connected to GPIO 0 (it's slice 0)

19     uint slice_num = pwm_gpio_to_slice_num(0);

20 

21     // Set period of 4 cycles (0 to 3 inclusive)

22     pwm_set_wrap(slice_num, 3);

23     // Set channel A output high for one cycle before dropping

24     pwm_set_chan_level(slice_num, PWM_CHAN_A, 1);

25     // Set initial B output high for three cycles before dropping

26     pwm_set_chan_level(slice_num, PWM_CHAN_B, 3);

27     // Set the PWM running

28     pwm_set_enabled(slice_num, true);

Figure 113 shows how the PWM hardware operates once it has been configured.

Count
0
1
2
3
0
1
2
3
0
1
2
3
Figure 113. The slice

|  | 0 | 1 | 2 | 3 | 0 | 1 | 2 | 3 | 0 | 1 | 2 | 3 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|  |  |  |  |  |  |  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |  |  |  |  |  |  |

counts repeatedly

from 0 to 3, which is

A

configured as the TOP

B

value. The output

waves therefore have

a period of 4. Output A

By default, PWM slices count upward until they reach the value of the TOP register. After they reach the TOP value, they

is high for 1 cycle in 4,

wrap to 0. Alternatively, set CSR_PH_CORRECT to 1 to enable phase-correct mode, where the counter counts downward after

so the average output

voltage is 1/4 of the

reaching TOP, until it reaches 0 again.

IO supply voltage.

Output B is high for 3

Phase-correct mode centres the pulse on the same point no matter the duty cycle; its phase is not a function of duty

cycles in every 4. The

cycle. When phase-correct mode is enabled, the output frequency is halved. The slice spends two cycles at a count of

rising edges of A and

TOP and two cycles at a count of 0 each PWM period.

B are always aligned.

12.5. PWM
1079

![Page 1081 figure](images/fig_p1081.png)

RP2350 Datasheet

Figure 114. In phase-

Input (Count)

correct mode, the

Count

counter counts back

TOP

down from TOP to 0

Counter compare level

once it reaches TOP.

Counter

TOP/3

|  |  |  |
| --- | --- | --- |
|  | T
Output (P | 2T
ulse) |
|  |  |  |

0

IOVDD

GPIO pulse output

0

T
2T
3T
t

12.5.2.2. 0% and 100% Duty Cycle

The RP2350 PWM can produce toggle-free 0% and 100% duty cycle output.

Figure 115. Glitch-free

Input (Count)
Count

0% duty cycle output

for CC = 0, and glitch-

TOP

free 100% duty cycle

Counter compare level

output for CC = TOP +

Counter

1

0

T
2T
3T
t

Output (Pulse)
V

IOVDD

GPIO pulse output

0

T
2T
3T
t

A CC value of 0 produces a 0% output: the output signal is always low. A CC value of TOP + 1 (equal to the period when not

phase-corrected) produces a 100% output. If TOP is 254, the counter has a period of 255 cycles, and CC values in the

range of 0 to 255 inclusive will produce duty cycles in the range 0% to 100% inclusive.

Glitch-free output at 0% and 100% helps avoid switching losses, for instance, when a MOSFET is controlled at its

minimum and maximum current levels.

12.5.2.3. Double buffering

Figure 116 shows how a change in input value produces a change in output duty cycle. This can approximate analogue

waveforms such as a sine wave.

12.5. PWM
1080

![Page 1082 figure](images/fig_p1082.png)

RP2350 Datasheet

Figure 116. The input

Input (Count)

value varies with each

Count

counter period: first

TOP

|  |  |  |  |  |
| --- | --- | --- | --- | --- |
|  |  |  |  |  |
|  |  |  |  |  |
|  | T | Output (P | 2
ulse) | T 3 |
|  |  |  |  |  |

TOP / 3, then 2 × TOP

Counter compare level

/ 3, and finally TOP + 1

Counter

2×TOP/3

for 100% duty cycle.

Each increase in the

TOP/3

input value causes a

corresponding

0

increase in the output

duty cycle.

IOVDD

GPIO pulse output

0

T
5T/3
T/3
2T
3T
t

In Figure 116, the input value only changes at the instant where the counter wraps through 0. Figure 117 shows what

happens if the input value is allowed to change at any other time: an unwanted glitch is produced at the output.

Figure 117. The input

Input (Count)

value changes whilst

Count

the counter is mid-

TOP

|  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  | T | Ou | tpu | t (P | 2
ulse) | T 3 |
|  |  |  |  |  |  |  |

ramp. This produces

Counter compare level

additional toggling at

Counter

2×TOP/3

the output.

TOP/3

0

IOVDD

GPIO pulse output

0

T
5T/3
T/3
2T
3T
t

The behaviour becomes even more perplexing if the TOP register is also modified. It would be difficult for software to

write to CC or TOP with the correct timing. To solve this, each slice has two copies of the CC and TOP registers: one copy

that software can modify, and another, internal copy that is updated from the first register at the instant the counter

wraps. Software can modify its copy of the register at will, but the changes are not captured by the PWM output until the

next wrap.

Figure 118 shows the sequence of events where a software interrupt handler changes the value of CC_A each time the

counter wraps.

12.5. PWM
1081

![Page 1083 figure](images/fig_p1083.png)

RP2350 Datasheet

Figure 118. Each

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
|  |  | 0 |  |  |  |  | 1 |  |  |  |  |  | 2 |  |  |  | 3 |  |
|  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
|  |  |  |  |  | 0 |  |  |  |  |  | 1 |  |  |  |  | 2 |  |  |

Counter at top

counter wrap causes

IRQ

the interrupt request

signal to assert. The

CC_A

processor enters its

interrupt handler,

writes to its copy of

the CC register, and

clears the interrupt.

There is no limitation on what values can be written to CC or TOP, or when they are written. In normal PWM mode

When the counter

(CSR_PH_CORRECT is 0), the latched copies update when the counter wraps to 0, which occurs once every TOP + 1 cycles. In

wraps again, the

phase-correct mode (CSR_PH_CORRECT is 1), the latched copies update on the 0 to 0 count transition, when the counter

latched version of the

CC register is

stops counting downward and begins to count upward again.

instantaneously

updated with the most

recent value written by

software, and this

12.5.2.4. Clock divider

value controls the duty

cycle for the next

period. The IRQ is

Each slice has a 8 integer bit, 4 fractional bit fractional clock divider configured by the DIV register. The clock divider

reasserted so that

allows you to slow the count rate by a factor of up to 256. To do this, the PWM generates an enable signal that gates

software can write

counter operation. This allows you to achieve output frequencies significantly lower than the system clock. For

another fresh value to

its copy of the CC

instance, from a 125MHz system clock, the clock divider can slow the count rate to approximately 7.5Hz. Lower

register.

frequencies than this require a system timer interrupt (Section 12.8).

Figure 119. The clock

|  |  |  |  |  | 1 |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|  |  |  |  |  |  |  |  |  |  |  |
|  |  |  |  |  | .0 |  |  |  |  |  |
|  |  |  |  |  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |  |  |  |  |
|  |  |  |  |  | 3 |  |  |  |  |  |
|  |  |  |  |  |  |  |  |  |  |  |
|  |  |  |  |  | .0 |  |  |  |  |  |
|  |  |  |  |  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |  |  |  |  |
|  |  |  |  |  | 2 |  |  |  |  |  |
|  |  |  |  |  |  |  |  |  |  |  |
|  |  |  |  |  | .5 |  |  |  |  |  |
|  |  |  |  |  |  |  |  |  |  |  |

divider generates an

enable signal. The

counter only counts on

cycles where this

Counter enable

signal is high. A clock

divisor of 1 causes the

enable to be asserted

on every cycle, so the

counter counts by one

Counter enable

on every system clock

cycle. Higher divisors

cause the count

enable to be asserted

less frequently.

Fractional division

Counter enable

achieves an average

fractional counting

rate by spacing some

The fractional divider is a first-order delta-sigma type.

enable pulses further

apart than others.

The clock divider also extends the effective count range when using level-sensitive or edge-sensitive modes to take duty

cycle or frequency measurements.

12.5.2.5. Level-sensitive and edge-sensitive triggering

The PWM provides the following counter modes:

• Default free-running, counting continuously whenever the slice is enabled (free-running)
• Count continuously when a high level is detected on the B pin (level sensitive)
• Count once with each rising edge detected on the B pin (rising edge-sensitive)
• Count once with each falling edge detected on the B pin (falling edge-sensitive)

12.5. PWM
1082

![Page 1084 figure](images/fig_p1084.png)

RP2350 Datasheet

Figure 120. PWM slice

Event select

Phase
Advance

Phase
Retard

event selection. The

counter advances

when its enable input

1

is high. This enable is

generated by two

Input 
(pin B)

sequential stages.

Count 
enable
Fractional Clock 

EN

First, any one of four

Divider (8.4)
Rising edge

event types (always

on, pin B high, pin B

rise, pin B fall) can

Falling edge

generate enable

pulses for the

fractional clock

divider. The divider

Use the DIVMODE field in each slice’s CSR to select a mode. In free-running mode, the A and B pins are both outputs. In any

can reduce the rate of

other mode, the B pin becomes an input that controls counter operation. CC_B is ignored when not in free-running mode.

the enable pulses,

before passing them

You can measure the duty cycle or frequency of an input signal by running the slice for a fixed amount of time in level-

on to the counter.

sensitive or edge-sensitive mode. Due to the type of edge-detect circuit used, the low period and high period of the

measured signal must both be strictly greater than the system clock period when taking frequency measurements.

The clock divider still operates in level-sensitive and edge-sensitive modes. At maximum division (DIV_INT is 0), the

counter only advances once per 256 high input cycles in level-sensitive modes, or once per 256 edges in edge-sensitive

mode. This allows you to take longer-running measurements, although the resolution is still 16 bits.

Pico Examples: https://github.com/raspberrypi/pico-examples/blob/master/pwm/measure_duty_cycle/measure_duty_cycle.c Lines 19 - 37

19 float measure_duty_cycle(uint gpio) {

20     // Only the PWM B pins can be used as inputs.

21     assert(pwm_gpio_to_channel(gpio) == PWM_CHAN_B);

22     uint slice_num = pwm_gpio_to_slice_num(gpio);

23 

24     // Count once for every 100 cycles the PWM B input is high

25     pwm_config cfg = pwm_get_default_config();

26     pwm_config_set_clkdiv_mode(&cfg, PWM_DIV_B_HIGH);

27     pwm_config_set_clkdiv(&cfg, 100);

28     pwm_init(slice_num, &cfg, false);

29     gpio_set_function(gpio, GPIO_FUNC_PWM);

30 

31     pwm_set_enabled(slice_num, true);

32     sleep_ms(10);

33     pwm_set_enabled(slice_num, false);

34     float counting_rate = clock_get_hz(clk_sys) / 100;

35     float max_possible_count = counting_rate * 0.01;

36     return pwm_get_counter(slice_num) / max_possible_count;

37 }

12.5.2.6. Configuring PWM period

When free-running, use the following three parameters to control the period of a PWM slice’s output (measured in

system clock cycles):

• The TOP register, which controls the maximum value of the counting period
• The CSR_PH_CORRECT bit, which enables phase-correct mode
• The DIV register, which controls the clock divider

The slice counts from 0 to TOP, then either wraps or begins counting backward, depending on the setting of

CSR_PH_CORRECT. The clock divider slows the rate of counting, with a maximum speed of one count per cycle, and a

minimum speed of one count per 256 cycles. Calculate the period in clock cycles with the following equation:

12.5. PWM
1083

RP2350 Datasheet

To determine the output frequency based on the system clock frequency, use the following equation:

Set DIV_INT to 0 to divide the count rate by the maximum possible value of 256. You must not set any DIV_FRAC bits when

DIV_INT is 0.

12.5.2.7. Interrupt Request (IRQ) and DMA Data Request (DREQ)

The PWM block has two IRQ outputs. The interrupt status registers INTR, INTS0, INTS1, INTE0 and INTE1 allow software to:

• Control which slices assert each of the two IRQs
• Check which slices caused the assertion of an IRQ
• Clear and acknowledge the interrupt

A slice generates an interrupt request each time its counter wraps (or, in phase-correct mode, each time the counter

returns to 0). This sets the flag corresponding to this slice in the raw interrupt status register, INTR. If this slice’s interrupt

is enabled in INTE, this flag causes the PWM block’s IRQ to be asserted, and the flag also appears in the masked

interrupt status register INTS.

To clear flags, write a mask back to INTR. This is demonstrated in the LED fade SDK example below:

Pico Examples: https://github.com/raspberrypi/pico-examples/blob/master/pwm/led_fade/pwm_led_fade.c

 1 /**

 2  * Copyright (c) 2020 Raspberry Pi (Trading) Ltd.

 3  *

 4  * SPDX-License-Identifier: BSD-3-Clause

 5  */

 6 

 7 // Fade an LED between low and high brightness. An interrupt handler updates

 8 // the PWM slice's output level each time the counter wraps.

 9 

10 #include "pico/stdlib.h"

11 #include <stdio.h>

12 #include "pico/time.h"

13 #include "hardware/irq.h"

14 #include "hardware/pwm.h"

15 

16 void on_pwm_wrap() {

17     static int fade = 0;

18     static bool going_up = true;

19     // Clear the interrupt flag that brought us here

20     pwm_clear_irq(pwm_gpio_to_slice_num(PICO_DEFAULT_LED_PIN));

21 

22     if (going_up) {

23         ++fade;

24         if (fade > 255) {

25             fade = 255;

26             going_up = false;

27         }

28     } else {

29         --fade;

30         if (fade < 0) {

31             fade = 0;

32             going_up = true;

12.5. PWM
1084

RP2350 Datasheet

33         }

34     }

35     // Square the fade value to make the LED's brightness appear more linear

36     // Note this range matches with the wrap value

37     pwm_set_gpio_level(PICO_DEFAULT_LED_PIN, fade * fade);

38 }

39 

40 int main() {

41 #ifndef PICO_DEFAULT_LED_PIN

42 #warning pwm/led_fade example requires a board with a regular LED

43 #else

44     // Tell the LED pin that the PWM is in charge of its value.

45     gpio_set_function(PICO_DEFAULT_LED_PIN, GPIO_FUNC_PWM);

46     // Figure out which slice we just connected to the LED pin

47     uint slice_num = pwm_gpio_to_slice_num(PICO_DEFAULT_LED_PIN);

48 

49     // Mask our slice's IRQ output into the PWM block's single interrupt line,

50     // and register our interrupt handler

51     pwm_clear_irq(slice_num);

52     pwm_set_irq_enabled(slice_num, true);

53     irq_set_exclusive_handler(PWM_DEFAULT_IRQ_NUM(), on_pwm_wrap);

54     irq_set_enabled(PWM_DEFAULT_IRQ_NUM(), true);

55 

56     // Get some sensible defaults for the slice configuration. By default, the

57     // counter is allowed to wrap over its maximum range (0 to 2**16-1)

58     pwm_config config = pwm_get_default_config();

59     // Set divider, reduces counter clock to sysclock/this value

60     pwm_config_set_clkdiv(&config, 4.f);

61     // Load the configuration into our PWM slice, and set it running.

62     pwm_init(slice_num, &config, true);

63 

64     // Everything after this point happens in the PWM interrupt handler, so we

65     // can twiddle our thumbs

66     while (1)

67         tight_loop_contents();

68 #endif

69 }

This scheme allows multiple slices to generate interrupts concurrently. A system interrupt handler determines which

slices caused the most recent interruption, and handles them appropriately. Normally, this means reloading those slices'

TOP or CC registers, but the PWM block can also be used as a source of regular interrupt requests for non-PWM purposes.

The same pulse which sets the interrupt flag in INTR is also available as a one-cycle data request to the RP2350 system

DMA. For each cycle the DMA sees a DREQ asserted, it makes one data transfer to its programmed location in as timely

a manner as possible. Combined with the double-buffered behaviour of CC and TOP, the DMA can efficiently stream data

to a PWM slice at a rate of one transfer per counter period. Alternatively, a PWM slice could serve as a pacing timer for

DMA transfers to some other memory-mapped hardware.

12.5.2.8. On-the-fly phase adjustment

For some applications, it is necessary to control the phase relationship between two PWM outputs on different slices.

The global enable register EN contains an alias of the CSR_EN flag for each slice. Use this register to start and stop several

slices simultaneously. If two slices with the same output frequency start at the same time, they run in perfect lockstep,

with a fixed phase relationship determined by the initial counter values.

The CSR_PH_ADV and CSR_PH_RET fields advance or retard a slice’s output phase by one count whilst it is running. They do so

by inserting or deleting pulses from the clock enable (the output of the clock divider), as shown in Figure 121.

12.5. PWM
1085

## Embedded Images

![img_p1085_00.png](images/img_p1085_00.png)

![img_p1085_01.png](images/img_p1085_01.png)

