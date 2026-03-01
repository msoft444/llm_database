---
source_pdf: rp2350-datasheet-7.pdf
repository: llm_database
chapter: Chapter 11. PIO
section: 11.5.4. Autopush and Autopull
pages: 907-911
type: technical_spec
generated_at: 2026-03-01T06:07:53.318745+00:00
---

# 11.5.4. Autopush and Autopull

11.5.4. Autopush and Autopull

With each OUT instruction, the OSR gradually empties, as data is shifted out. Once empty, it must be refilled: for example,

a PULL transfers one word of data from the TX FIFO to the OSR. Similarly, the ISR must be emptied once full. One

approach to this is a loop which performs a PULL after an appropriate amount of data has been shifted:

 1 .program manual_pull

 2 .side_set 1 opt

 3 

 4 .wrap_target

 5     set x, 2                   ; X = bit count - 2

 6     pull            side 1 [1]  ; Stall here if no TX data

 7 bitloop:

 8     out pins, 1     side 0 [1]  ; Shift out data bit and toggle clock low

 9     jmp x-- bitloop side 1 [1]  ; Loop runs 3 times

10     out pins, 1     side 0      ; Shift out last bit before reloading X

11 .wrap

This program shifts out 4 bits from each FIFO word, with an accompanying bit clock, at a constant rate of 1 bit per 4

cycles. When the TX FIFO is empty, it stalls with the clock high (noting that side-set still takes place on cycles where the

11.5. Functional details
906

RP2350 Datasheet

instruction stalls). Figure 49 shows how a state machine would execute this program.

![Page 908 figure](images/fig_p0908.png)

Figure 49. Execution

of manual_pull

PULL
SET
OUT
JMP
OUT
JMP
OUT
JMP
SET
OUT
PULL

program. X is used as

2
1
0
2
-1

a loop counter. On

Bit 0
Bit 1
Bit 2
Bit 3
Data pin (OUT)

each iteration, one

data bit is shifted out,

and the clock is

0
2
3
4
32

asserted low, then

high. A delay cycle on

each instruction

This program has some limitations:

brings the total up to

• It occupies 5 instruction slots, but only 2 of these are immediately useful (out pins, 1 set 0 and … set 1), for

four cycles per

iteration. After the

outputting serial data and a clock.
• Throughput is limited to system clock over 4, due to the extra cycles required to pull in new data, and reload the

third loop, a fourth bit

is shifted out, and the

state machine

immediately returns to

the start of the

This is a common type of problem for PIO, so each state machine has some extra hardware to handle it. State machines

program to reload the

loop counter and pull

keep track of the total shift count OUT of the OSR and IN to the ISR, and trigger certain actions once these counters reach

fresh data, while

maintaining the 4

cycles/bit cadence.

• On an OUT instruction which reaches or exceeds the pull threshold, the state machine can simultaneously refill the

OSR from the TX FIFO, if data is available.
• On an IN instruction which reaches or exceeds the push threshold, the state machine can write the shift result

directly to the RX FIFO, and clear the ISR.

The manual_pull example can be rewritten to take advantage of automatic pull (autopull):

5     out pins, 1   side 0    [1]

6     nop           side 1    [1]

This is shorter and simpler than the original, and can run twice as fast, if the delay cycles are removed, since the

hardware refills the OSR "for free". Note that the program does not determine the total number of bits to be shifted

before the next pull; the hardware automatically pulls once the programmable threshold, SHIFCTRL_PULL_THRESH, is reached,

so the same program could also shift out e.g. 16 or 32 bits from each FIFO word.

Finally, note that the above program is not exactly the same as the original, since it stalls with the clock output low,

rather than high. We can change the location of the stall, using the PULL IFEMPTY instruction, which uses the same

configurable threshold as autopull:

1 .program somewhat_manual_pull

5     out pins, 1   side 0    [1]

6     pull ifempty  side 1    [1]

Below is a complete example (PIO program, plus a C program to load and run it) which illustrates autopull and autopush

both enabled on the same state machine. It programs state machine 0 to loopback data from the TX FIFO to the RX

FIFO, with a throughput of one word per two clocks. It also demonstrates how the state machine will stall if it tries to OUT

when both the OSR and TX FIFO are empty.

11.5. Functional details
907

RP2350 Datasheet

1 .program auto_push_pull

2 

3 .wrap_target

4     out x, 32

5     in x, 32

6 .wrap

 1 #include "tb.h" // TODO this is built against existing sw tree, so that we get printf etc

 2 

 3 #include "platform.h"

 4 #include "pio_regs.h"

 5 #include "system.h"

 6 #include "hardware.h"

 7 

 8 #include "auto_push_pull.pio.h"

 9 

10 int main()

11 {

12     tb_init();

13 

14     // Load program and configure state machine 0 for autopush/pull with

15     // threshold of 32, and wrapping on program boundary. A threshold of 32 is

16     // encoded by a register value of 00000.

17     for (int i = 0; i < count_of(auto_push_pull_program); ++i)

18         mm_pio->instr_mem[i] = auto_push_pull_program[i];

19     mm_pio->sm[0].shiftctrl =

20         (1u << PIO_SM0_SHIFTCTRL_AUTOPUSH_LSB) |

21         (1u << PIO_SM0_SHIFTCTRL_AUTOPULL_LSB) |

22         (0u << PIO_SM0_SHIFTCTRL_PUSH_THRESH_LSB) |

23         (0u << PIO_SM0_SHIFTCTRL_PULL_THRESH_LSB);

24     mm_pio->sm[0].execctrl = 

25         (auto_push_pull_wrap_target << PIO_SM0_EXECCTRL_WRAP_BOTTOM_LSB) |

26         (auto_push_pull_wrap << PIO_SM0_EXECCTRL_WRAP_TOP_LSB);

27 

28     // Start state machine 0

29     hw_set_bits(&mm_pio->ctrl, 1u << (PIO_CTRL_SM_ENABLE_LSB + 0));

30 

31     // Push data into TX FIFO, and pop from RX FIFO

32     for (int i = 0; i < 5; ++i)

33         mm_pio->txf[0] = i;

34     for (int i = 0; i < 5; ++i)

35         printf("%d\n", mm_pio->rxf[0]);

36 

37     return 0;

38 }

Figure 50 shows how the state machine executes the example program. Initially the OSR is empty, so the state machine

stalls on the first OUT instruction. Once data is available in the TX FIFO, the state machine transfers this into the OSR. On

the next cycle, the OUT can execute using the data in the OSR (in this case, transferring this data to the X scratch

register), and the state machine simultaneously refills the OSR with fresh data from the FIFO. Since every IN instruction

immediately fills the ISR, the ISR remains empty, and IN transfers data directly from scratch X to the RX FIFO.

11.5. Functional details
908

RP2350 Datasheet

![Page 910 figure](images/fig_p0910.png)

Figure 50. Execution

of auto_push_pull

IN
OUT
OUT
IN
OUT
IN
OUT
IN
IN
OUT
OUT

program. The state

machine stalls on an

OUT until data has

travelled through the

TX FIFO into the OSR.

0
0
0
0
32
32
0

Subsequently, the OSR

is refilled

simultaneously with

0
0
0
0
0
0

each OUT operation

2
3
4
5
1

(due to bit count of

32), and IN data

To trigger automatic push or pull at the correct time, the state machine tracks the total shift count of the ISR and OSR,

bypasses the ISR and

goes straight to the RX

using a pair of saturating 6-bit counters.

FIFO. The state

• At reset, or upon CTRL_SM_RESTART assertion, ISR shift counter is set to 0 (nothing shifted in), and OSR to 32 (nothing

machine stalls again

when the FIFO has

left to be shifted out)
• An OUT instruction increases the OSR shift counter by Bit count
• An IN instruction increases the ISR shift counter by Bit count
• A PULL instruction or autopull clears the OSR counter to 0
• A PUSH instruction or autopush clears the ISR counter to 0
• A MOV OSR, x or MOV ISR, x clears the OSR or ISR shift counter to 0, respectively
• A OUT ISR, n instruction sets the ISR shift counter to n

drained, and the OSR

is once again empty.

On any OUT or IN instruction, the state machine compares the shift counters to the values of SHIFTCTRL_PULL_THRESH and

SHIFTCTRL_PUSH_THRESH to decide whether action is required. Autopull and autopush are individually enabled by the

SHIFTCTRL_AUTOPULL and SHIFTCTRL_AUTOPUSH fields.

Pseudocode for an IN with autopush enabled:

 1 isr = shift_in(isr, input())

 2 isr count = saturate(isr count + in count)

The hardware performs the above steps in a single machine clock cycle, unless there is a stall.

Threshold is configurable from 1 to 32.

11.5. Functional details
909

RP2350 Datasheet

IMPORTANT

Autopush must not be enabled when SHIFTCTRL_FJOIN_RX_PUT or SHIFTCTRL_FJOIN_RX_GET is set. Its operation in this state

is undefined.

11.5.4.2. Autopull Details

On non-OUT cycles, the hardware performs the equivalent of the following pseudocode:

1 if MOV or PULL:

2     osr count = 0

3 

4 if osr count >= threshold:

5     if tx fifo not empty:

6         osr = pull()

7         osr count = 0

An autopull can therefore occur at any point between two OUTs, depending on when the data arrives in the FIFO.

On OUT cycles, the sequence is a little different:

 1 if osr count >= threshold:

 2     if tx fifo not empty:

 3         osr = pull()

 4         osr count = 0

 5     stall

 6 else:

 7     output(osr)

 8     osr = shift(osr, out count)

 9     osr count = saturate(osr count + out count)

10 

11     if osr count >= threshold:

12         if tx fifo not empty:

13             osr = pull()

14             osr count = 0

The hardware is capable of refilling the OSR simultaneously with shifting out the last of the shift data, as these two

operations can proceed in parallel. However, it cannot fill an empty OSR and OUT it on the same cycle, due to the long

logic path this would create.

The refill is somewhat asynchronous to your program, but an OUT behaves as a data fence, and the state machine will

never OUT data which you didn’t write into the FIFO.

Note that a MOV from the OSR is undefined whilst autopull is enabled; you will read either any residual data that has not

been shifted out, or a fresh word from the FIFO, depending on a race against system DMA. Likewise, a MOV to the OSR

may overwrite data which has just been autopulled. However, data which you MOV into the OSR will never be overwritten,

since MOV updates the shift counter.

If you do need to read the OSR contents, you should perform an explicit PULL of some kind. The nondeterminism

described above is the cost of the hardware managing pulls automatically. When autopull is enabled, the behaviour of

PULL is altered: it becomes a no-op if the OSR is full. This is to avoid a race condition against the system DMA. It behaves

as a fence: either an autopull has already taken place, in which case the PULL has no effect, or the program will stall on

the PULL until data becomes available in the FIFO.

PULL does not require similar behaviour, because autopush does not have the same nondeterminism.

11.5. Functional details
910
