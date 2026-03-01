---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 11. PIO
section: 11.5.3. FIFO joining
pages: 906-906
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 11.5.3. FIFO joining

![Page 906 figure](images/fig_p0906.png)

RP2350 Datasheet

25 static const struct pio_program squarewave_wrap_program = {

26     .instructions = squarewave_wrap_program_instructions,

27     .length = 3,

28     .origin = -1,

29     .pio_version = squarewave_wrap_pio_version,

30     .used_gpio_ranges = 0x0

31 #endif

32 };

33 

34 static inline pio_sm_config squarewave_wrap_program_get_default_config(uint offset) {

35     pio_sm_config c = pio_get_default_sm_config();

36     sm_config_set_wrap(&c, offset + squarewave_wrap_wrap_target, offset +

   squarewave_wrap_wrap);

37     return c;

38 }

This is raw output from the PIO assembler, pioasm, which has created a default pio_sm_config object containing the WRAP

register values from the program listing. The control register fields could also be initialised directly.

NOTE

WRAP_BOTTOM and WRAP_TOP are absolute addresses in the PIO instruction memory. If a program is loaded at an offset,

the wrap addresses must be adjusted accordingly.

The squarewave_wrap example has delay cycles inserted, so that it behaves identically to the original squarewave program.

Thanks to program wrapping, these can now be removed, so that the output toggles twice as fast, while maintaining an

even balance of high and low periods.

Pico Examples: https://github.com/raspberrypi/pico-examples/blob/master/pio/squarewave/squarewave_fast.pio Lines 12 - 18

12 .program squarewave_fast

13 ; Like squarewave_wrap, but remove the delay cycles so we can run twice as fast.

14     set pindirs, 1   ; Set pin to output

15 .wrap_target

16     set pins, 1      ; Drive pin high

17     set pins, 0      ; Drive pin low

18 .wrap

11.5.3. FIFO joining

By default, each state machine possesses a 4-entry FIFO in each direction: one for data transfer from system to state

machine (TX), the other for the reverse direction (RX). However, many applications do not require bidirectional data

transfer between the system and an individual state machine, but may benefit from deeper FIFOs: in particular, high-

bandwidth interfaces such as DPI. For these cases, SHIFTCTRL_FJOIN can merge the two 4-entry FIFOs into a single 8-entry

FIFO.

11.5. Functional details
905
