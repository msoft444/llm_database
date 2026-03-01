---
source_pdf: rp2350-datasheet-9.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.6.9. Example use cases
pages: 1109-1113
type: technical_spec
generated_at: 2026-03-01T11:44:31.548839+00:00
---

# 12.6.9. Example use cases

12.6.9. Example use cases

12.6.9.1. Using interrupts to reconfigure a channel

When a channel finishes a block of transfers, it becomes available for making more transfers. Software detects that the

channel is no longer busy, and reconfigures and restarts the channel. One approach is to poll the CTRL_BUSY bit until the

channel is done, but this loses one of the key advantages of the DMA, namely that it does not have to operate in

lockstep with a processor. By setting the correct bit in INTE0 through INTE3, you can instruct the DMA to raise one of its

four interrupt request lines when a given channel completes. Rather than repeatedly asking if a channel is done, you are

told.

NOTE

Having four system interrupt lines allows different channel completion interrupts to be routed to different cores, or

to pre-empt one another on the same core if one channel is more time-critical. It also allows channel interrupts to

target different security domains.

When the interrupt is asserted, the processor can be configured to drop whatever it is doing and call a user-specified

handler function. The handler can reconfigure and restart the channel. When the handler exits, the processor returns to

the interrupted code running in the foreground.

Pico Examples: https://github.com/raspberrypi/pico-examples/blob/master/dma/channel_irq/channel_irq.c Lines 35 - 52

```c
35 void dma_handler() {
36     static int pwm_level = 0;
37     static uint32_t wavetable[N_PWM_LEVELS];
38     static bool first_run = true;
39     // Entry number `i` has `i` one bits and `(32 - i)` zero bits.
40     if (first_run) {
41         first_run = false;
42         for (int i = 0; i < N_PWM_LEVELS; ++i)
43             wavetable[i] = ~(~0u << i);
44     }
45 
```

12.6. DMA
1108

RP2350 Datasheet

```c
46     // Clear the interrupt request.
47     dma_hw->ints0 = 1u << dma_chan;
48     // Give the channel a new wave table entry to read from, and re-trigger it
49     dma_channel_set_read_addr(dma_chan, &wavetable[pwm_level], true);
50 
51     pwm_level = (pwm_level + 1) % N_PWM_LEVELS;
52 }
```

In many cases, most of the configuration can be done the first time the channel starts. This way, only addresses and

transfer lengths need reprogramming in the interrupt handler.

Pico Examples: https://github.com/raspberrypi/pico-examples/blob/master/dma/channel_irq/channel_irq.c Lines 54 - 94

```c
54 int main() {
55 #ifndef PICO_DEFAULT_LED_PIN
56 #warning dma/channel_irq example requires a board with a regular LED
57 #else
58     // Set up a PIO state machine to serialise our bits
59     uint offset = pio_add_program(pio0, &pio_serialiser_program);
60     pio_serialiser_program_init(pio0, 0, offset, PICO_DEFAULT_LED_PIN, PIO_SERIAL_CLKDIV);
61 
62     // Configure a channel to write the same word (32 bits) repeatedly to PIO0
63     // SM0's TX FIFO, paced by the data request signal from that peripheral.
64     dma_chan = dma_claim_unused_channel(true);
65     dma_channel_config c = dma_channel_get_default_config(dma_chan);
66     channel_config_set_transfer_data_size(&c, DMA_SIZE_32);
67     channel_config_set_read_increment(&c, false);
68     channel_config_set_dreq(&c, DREQ_PIO0_TX0);
69 
70     dma_channel_configure(
71         dma_chan,
72         &c,
73         &pio0_hw->txf[0], // Write address (only need to set this once)
74         NULL,             // Don't provide a read address yet
75         PWM_REPEAT_COUNT, // Write the same value many times, then halt and interrupt
76         false             // Don't start yet
77     );
78 
79     // Tell the DMA to raise IRQ line 0 when the channel finishes a block
80     dma_channel_set_irq0_enabled(dma_chan, true);
81 
82     // Configure the processor to run dma_handler() when DMA IRQ 0 is asserted
83     irq_set_exclusive_handler(DMA_IRQ_0, dma_handler);
84     irq_set_enabled(DMA_IRQ_0, true);
85 
86     // Manually call the handler once, to trigger the first transfer
87     dma_handler();
88 
89     // Everything else from this point is interrupt-driven. The processor has
90     // time to sit and think about its early retirement -- maybe open a bakery?
91     while (true)
92         tight_loop_contents();
93 #endif
94 }
```

One disadvantage of this technique is that you don’t start to reconfigure the channel until some time after the channel

makes its last transfer. If there is heavy interrupt activity on the processor, this can be quite a long time, and quite a

large gap in transfers. This makes it difficult to sustain a high data throughput.

This is solved by using two channels, with their CHAIN_TO fields crossed over, so that channel A triggers channel B when it

completes, and vice versa. At any point in time, one of the channels is transferring data. The other is either already

12.6. DMA
1109

RP2350 Datasheet

configured to start the next transfer immediately when the current one finishes, or it is in the process of being

reconfigured. When channel A completes, it immediately starts the cued-up transfer on channel B. At the same time, the

interrupt is fired, and the handler reconfigures channel A so that it is ready when channel B completes.

12.6.9.2. DMA control blocks

Frequently, multiple smaller buffers must be gathered together and sent to the same peripheral. To address this use

case, the RP2350 DMA can execute a long and complex sequence of transfers without processor control. One channel

repeatedly reconfigures a second channel, and the second channel restarts the first each time it completes block of

transfers.

Because the first DMA channel transfers data directly from memory to the second channel’s control registers, the

format of the control blocks in memory must match those registers. Each time, the last register written to will be one of

the trigger registers (Section 12.6.3.1), which will start the second channel on its programmed block of transfers. The

register aliases (Section 12.6.3.1) give some flexibility for the block layout, and more importantly allow some registers

to be omitted from the blocks, so they occupy less memory and can be loaded more quickly.

This example shows how multiple buffers can be gathered and transferred to the UART, by reprogramming TRANS_COUNT

and READ_ADDR_TRIG:

Pico Examples: https://github.com/raspberrypi/pico-examples/blob/master/dma/control_blocks/control_blocks.c

```c
  1 /**
  2  * Copyright (c) 2020 Raspberry Pi (Trading) Ltd.
  3  *
  4  * SPDX-License-Identifier: BSD-3-Clause
  5  */
  6 
  7 // Use two DMA channels to make a programmed sequence of data transfers to the
  8 // UART (a data gather operation). One channel is responsible for transferring
  9 // the actual data, the other repeatedly reprograms that channel.
 10 
 11 #include <stdio.h>
 12 #include "pico/stdlib.h"
 13 #include "hardware/dma.h"
 14 #include "hardware/structs/uart.h"
 15 
 16 // These buffers will be DMA'd to the UART, one after the other.
 17 
 18 const char word0[] = "Transferring ";
 19 const char word1[] = "one ";
 20 const char word2[] = "word ";
 21 const char word3[] = "at ";
 22 const char word4[] = "a ";
 23 const char word5[] = "time.\n";
 24 
 25 // Note the order of the fields here: it's important that the length is before
 26 // the read address, because the control channel is going to write to the last
 27 // two registers in alias 3 on the data channel:
 28 //           +0x0        +0x4          +0x8          +0xC (Trigger)
 29 // Alias 0:  READ_ADDR   WRITE_ADDR    TRANS_COUNT   CTRL
 30 // Alias 1:  CTRL        READ_ADDR     WRITE_ADDR    TRANS_COUNT
 31 // Alias 2:  CTRL        TRANS_COUNT   READ_ADDR     WRITE_ADDR
 32 // Alias 3:  CTRL        WRITE_ADDR    TRANS_COUNT   READ_ADDR
 33 //
 34 // This will program the transfer count and read address of the data channel,
 35 // and trigger it. Once the data channel completes, it will restart the
 36 // control channel (via CHAIN_TO) to load the next two words into its control
 37 // registers.
 38 
 39 const struct {uint32_t len; const char *data;} control_blocks[] = {
```

12.6. DMA
1110

RP2350 Datasheet

```c
 40     {count_of(word0) - 1, word0}, // Skip null terminator
 41     {count_of(word1) - 1, word1},
 42     {count_of(word2) - 1, word2},
 43     {count_of(word3) - 1, word3},
 44     {count_of(word4) - 1, word4},
 45     {count_of(word5) - 1, word5},
 46     {0, NULL}                     // Null trigger to end chain.
 47 };
 48 
 49 int main() {
 50 #ifndef uart_default
 51 #warning dma/control_blocks example requires a UART
 52 #else
 53     stdio_init_all();
 54     puts("DMA control block example:");
 55 
 56     // ctrl_chan loads control blocks into data_chan, which executes them.
 57     int ctrl_chan = dma_claim_unused_channel(true);
 58     int data_chan = dma_claim_unused_channel(true);
 59 
 60     // The control channel transfers two words into the data channel's control
 61     // registers, then halts. The write address wraps on a two-word
 62     // (eight-byte) boundary, so that the control channel writes the same two
 63     // registers when it is next triggered.
 64 
 65     dma_channel_config c = dma_channel_get_default_config(ctrl_chan);
 66     channel_config_set_transfer_data_size(&c, DMA_SIZE_32);
 67     channel_config_set_read_increment(&c, true);
 68     channel_config_set_write_increment(&c, true);
 69     channel_config_set_ring(&c, true, 3); // 1 << 3 byte boundary on write ptr
 70 
 71     dma_channel_configure(
 72         ctrl_chan,
 73         &c,
 74         &dma_hw->ch[data_chan].al3_transfer_count, // Initial write address
 75         &control_blocks[0],                        // Initial read address
 76         2,                                         // Halt after each control block
 77         false                                      // Don't start yet
 78     );
 79 
 80     // The data channel is set up to write to the UART FIFO (paced by the
 81     // UART's TX data request signal) and then chain to the control channel
 82     // once it completes. The control channel programs a new read address and
 83     // data length, and retriggers the data channel.
 84 
 85     c = dma_channel_get_default_config(data_chan);
 86     channel_config_set_transfer_data_size(&c, DMA_SIZE_8);
 87     channel_config_set_dreq(&c, uart_get_dreq(uart_default, true));
 88     // Trigger ctrl_chan when data_chan completes
 89     channel_config_set_chain_to(&c, ctrl_chan);
 90     // Raise the IRQ flag when 0 is written to a trigger register (end of chain):
 91     channel_config_set_irq_quiet(&c, true);
 92 
 93     dma_channel_configure(
 94         data_chan,
 95         &c,
 96         &uart_get_hw(uart_default)->dr,
 97         NULL,           // Initial read address and transfer count are unimportant;
 98         0,              // the control channel will reprogram them each time.
 99         false           // Don't start yet.
100     );
101 
102     // Everything is ready to go. Tell the control channel to load the first
103     // control block. Everything is automatic from here.
```

12.6. DMA
1111

RP2350 Datasheet

```c
104     dma_start_channel_mask(1u << ctrl_chan);
105 
106     // The data channel will assert its IRQ flag when it gets a null trigger,
107     // indicating the end of the control block list. We're just going to wait
108     // for the IRQ flag instead of setting up an interrupt handler.
109     while (!(dma_hw->intr & 1u << data_chan))
110         tight_loop_contents();
111     dma_hw->ints0 = 1u << data_chan;
112 
113     puts("DMA finished.");
114 #endif
115 }
```
