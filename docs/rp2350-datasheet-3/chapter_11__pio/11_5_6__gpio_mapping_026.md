---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 11. PIO
section: 11.5.6. GPIO mapping
pages: 912-913
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 11.5.6. GPIO mapping

![Page 912 figure](images/fig_p0912.png)

RP2350 Datasheet

11.5.5. Clock Dividers

PIO runs off the system clock, but this is too fast for many interfaces, and the number of Delay cycles which can be

inserted is limited. Some devices, such as UART, require the signalling rate to be precisely controlled and varied, and

ideally multiple state machines can be varied independently while running identical programs. Each state machine is

equipped with a clock divider, for this purpose.

Rather than slowing the system clock itself, the clock divider redefines how many system clock periods are considered

to be "one cycle", for execution purposes. It does this by generating a clock enable signal, which can pause and resume

execution on a per-system-clock-cycle basis. The clock divider generates clock enable pulses at regular intervals, so

that the state machine runs at some steady pace, potentially much slower than the system clock.

Implementing the clock dividers in this way allows interfacing between the state machines and the system to be

simpler, lower-latency, and with a smaller footprint. The state machine is completely idle on cycles where clock enable

is low, though the system can still access the state machine’s FIFOs and change its configuration.

The clock dividers are 16-bit integer, 8-bit fractional, with first-order delta-sigma for the fractional divider. The clock

divisor can vary between 1 and 65536, in increments of 
.

If the clock divisor is set to 1, the state machine runs on every cycle, i.e. full speed:

Figure 51. State

System Clock

machine operation

1

CLKDIV_INT

with a clock divisor of

.0

CLKDIV_FRAC

1. Once the state

CTRL_SM_ENABLE

machine is enabled via

Clock Enable

the CTRL register, its

clock enable is

asserted on every

cycle.
In general, an integer clock divisor of n will cause the state machine to run 1 cycle in every n, giving an effective clock

speed of 
.

Figure 52. Integer

System Clock

clock divisors yield a

2

CLKDIV_INT

periodic clock enable.

.0

CLKDIV_FRAC

The clock divider

CTRL_SM_ENABLE

repeatedly counts

Clock Enable

down from n, and

emits an enable pulse

when it reaches 1.
Fractional division will maintain a steady state division rate of 
, where n and f are the integer and fractional

fields of this state machine’s CLKDIV register. It does this by selectively extending some division periods from 
 cycles to

.

Figure 53. Fractional

System Clock

clock division with an

2

CLKDIV_INT

average divisor of 2.5.

.5

CLKDIV_FRAC

The clock divider

CTRL_SM_ENABLE

maintains a running

Clock Enable

total of the fractional

value from each

division period, and

For small n, the jitter introduced by a fractional divider may be unacceptable. However, for larger values, this effect is

every time this value

much less apparent.

wraps through 1, the

integer divisor is

NOTE

increased by one for

the next division

period.

For fast asynchronous serial, it is recommended to use even divisions or multiples of 1 Mbaud where possible,

rather than the traditional multiples of 300, to avoid unnecessary jitter.

11.5.6. GPIO mapping

Internally, PIO has a 32-bit register for the output levels of each GPIO it can drive, and another register for the output

enables (Hi/Lo-Z). On every system clock cycle, each state machine can write to some or all of the GPIOs in each of

these registers.

11.5. Functional details
911

![Page 913 figure](images/fig_p0913.png)

RP2350 Datasheet

Figure 54. The state

machine has two

independent output

channels, one shared

by OUT/SET, and

another used by side-

set (which can happen

at any time). Three

independent mappings

(first GPIO, number of

GPIOs) control which

GPIOs OUT, SET and

side-set are directed

to. Input data is

rotated according to

which GPIO is mapped

to the LSB of the IN

data.

The write data and write masks for the output level and output enable registers come from the following sources:

• An OUT instruction writes to up to 32 bits. Depending on the instruction’s Destination field, this is applied to either

pins or pindirs. The least-significant bit of OUT data is mapped to PINCTRL_OUT_BASE, and this mapping continues for

PINCTRL_OUT_COUNT bits, wrapping after GPIO31.
• A SET instruction writes up to 5 bits. Depending on the instruction’s Destination field, this is applied to either pins or

pindirs. The least-significant bit of SET data is mapped to PINCTRL_SET_BASE, and this mapping continues for

PINCTRL_SET_COUNT bits, wrapping after GPIO31.
• A side-set operation writes up to 5 bits. Depending on the register field EXECCTRL_SIDE_PINDIR, this is applied to either

pins or pindirs. The least-significant bit of side-set data is mapped to PINCTRL_SIDESET_BASE, continuing for

PINCTRL_SIDESET_COUNT pins, minus one if EXECCTRL_SIDE_EN is set.

Each OUT/SET/side-set operation writes to a contiguous range of pins, but each of these ranges is independently sized

and positioned in the 32-bit GPIO space. This is sufficiently flexible for many applications. For example, if one state

machine is implementing some interface such as an SPI on a group of pins, another state machine can run the same

program, mapped to a different group of pins, and provide a second SPI interface.

On any given clock cycle, the state machine may perform an OUT or a SET, and may simultaneously perform a side-set.

The pin mapping logic generates a 32-bit write mask and write data bus for the output level and output enable registers,

based on this request, and the pin mapping configuration.

If a side-set overlaps with an OUT/SET performed by that state machine on the same cycle, the side-set takes precedence

in the overlapping region.

11.5.6.1. Output priority

Figure 55. Per-GPIO

priority select of write

masks from each

state machine. Each

GPIO considers level

and direction writes

from each of the four

state machines, and

applies the value from

the highest-numbered

state machine.

Each state machine may assert an OUT/SET and a side-set through its pin mapping hardware on each cycle. This

generates 32 bits of write data and write mask for the GPIO output level and output enable registers, from each state

machine.

For each GPIO, PIO collates the writes from all four state machines, and applies the write from the highest-numbered

11.5. Functional details
912

## Embedded Images

![img_p0912_00.png](images/img_p0912_00.png)

![img_p0912_01.png](images/img_p0912_01.png)

![img_p0912_02.png](images/img_p0912_02.png)

![img_p0912_03.png](images/img_p0912_03.png)

![img_p0912_04.png](images/img_p0912_04.png)

