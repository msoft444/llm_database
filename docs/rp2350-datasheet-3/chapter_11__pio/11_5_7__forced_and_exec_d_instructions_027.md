---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 11. PIO
section: 11.5.7. Forced and EXEC’d instructions
pages: 914-915
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 11.5.7. Forced and EXEC’d instructions

![Page 914 figure](images/fig_p0914.png)

RP2350 Datasheet

state machine. This occurs separately for output levels and output values — it is possible for a state machine to change

both the level and direction of the same pin on the same cycle (e.g. via simultaneous SET and side-set), or for one state

machine to change a GPIO’s direction while another changes that GPIO’s level. If no state machine asserts a write to a

GPIO’s level or direction, the value does not change.

11.5.6.2. Input mapping

The data observed by IN instructions is mapped such that the LSB is the GPIO selected by PINCTRL_IN_BASE, and

successively more-significant bits come from successively higher-numbered GPIOs, wrapping after 31.

In other words, the IN bus is a right-rotate of the GPIO input values, by PINCTRL_IN_BASE. If fewer than 32 GPIOs are

present, the PIO input is padded with zeroes up to 32 bits.

Some instructions, such as WAIT GPIO, use an absolute GPIO number, rather than an index into the IN data bus. In this

case, the right-rotate is not applied.

11.5.6.3. Input synchronisers

To protect PIO from metastabilities, each GPIO input is equipped with a standard 2-flipflop synchroniser. This adds two

cycles of latency to input sampling, but the benefit is that state machines can perform an IN PINS at any point, and will

see only a clean high or low level, not some intermediate value that could disturb the state machine circuitry. This is

absolutely necessary for asynchronous interfaces such as UART RX.

It is possible to bypass these synchronisers, on a per-GPIO basis. This reduces input latency, but it is then up to the user

to guarantee that the state machine does not sample its inputs at inappropriate times. Generally this is only possible for

synchronous interfaces such as SPI. Synchronisers are bypassed by setting the corresponding bit in INPUT_SYNC_BYPASS.

WARNING

Sampling a metastable input can lead to unpredictable state machine behaviour. This should be avoided.

11.5.7. Forced and EXEC’d instructions

Besides the instruction memory, state machines can execute instructions from 3 other sources:

• MOV EXEC which executes an instruction from some register Source
• OUT EXEC which executes data shifted out from the OSR
• The SMx_INSTR control registers, to which the system can write instructions for immediate execution

 1 .program exec_example

 2 

 3 hang:

 4     jmp hang

 5 execute:

 6     out exec, 32

 7     jmp execute

 8 

 9 .program instructions_to_push

10 

11     out x, 32

12     in x, 32

13     push

11.5. Functional details
913

RP2350 Datasheet

 1 #include "tb.h" // TODO this is built against existing sw tree, so that we get printf etc

 2 

 3 #include "platform.h"

 4 #include "pio_regs.h"

 5 #include "system.h"

 6 #include "hardware.h"

 7 

 8 #include "exec_example.pio.h"

 9 

10 int main()

11 {

12     tb_init();

13 

14     for (int i = 0; i < count_of(exec_example_program); ++i)

15         mm_pio->instr_mem[i] = exec_example_program[i];

16 

17     // Enable autopull, threshold of 32

18     mm_pio->sm[0].shiftctrl = (1u << PIO_SM0_SHIFTCTRL_AUTOPULL_LSB);

19 

20     // Start state machine 0 -- will sit in "hang" loop

21     hw_set_bits(&mm_pio->ctrl, 1u << (PIO_CTRL_SM_ENABLE_LSB + 0));

22 

23     // Force a jump to program location 1

24     mm_pio->sm[0].instr = 0x0000 | 0x1; // jmp execute

25 

26     // Feed a mixture of instructions and data into FIFO

27     mm_pio->txf[0] = instructions_to_push_program[0]; // out x, 32

28     mm_pio->txf[0] = 12345678;                        // data to be OUTed

29     mm_pio->txf[0] = instructions_to_push_program[1]; // in x, 32

30     mm_pio->txf[0] = instructions_to_push_program[2]; // push

31 

32     // The program pushed into TX FIFO will return some data in RX FIFO

33     while (mm_pio->fstat & (1u << PIO_FSTAT_RXEMPTY_LSB))

34         ;

35 

36     printf("%d\n", mm_pio->rxf[0]);

37 

38     return 0;

39 }

Here we load an example program into the state machine, which does two things:

• Enters an infinite loop
• Enters a loop which repeatedly pulls 32 bits of data from the TX FIFO, and executes the lower 16 bits as an

instruction

The C program sets the state machine running, at which point it enters the hang loop. While the state machine is still

running, the C program forces in a jmp instruction, which causes the state machine to break out of the loop.

When an instruction is written to the INSTR register, the state machine immediately decodes and executes that

instruction, rather than the instruction it would have fetched from the PIO’s instruction memory. The program counter

does not advance, so on the next cycle (assuming the instruction forced into the INSTR interface did not stall) the state

machine continues to execute its current program from the point where it left off, unless the written instruction itself

manipulated PC.

Delay cycles are ignored on instructions written to the INSTR register, and execute immediately, ignoring the state

machine clock divider. This interface is provided for performing initial setup and effecting control flow changes, so it

executes instructions in a timely manner, no matter how the state machine is configured.

Instructions written to the INSTR register are permitted to stall, in which case the state machine will latch this instruction

internally until it completes. This is signified by the EXECCTRL_EXEC_STALLED flag. This can be cleared by restarting the state

11.5. Functional details
914
