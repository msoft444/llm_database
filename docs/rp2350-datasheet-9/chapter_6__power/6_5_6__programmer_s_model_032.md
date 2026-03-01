---
source_pdf: rp2350-datasheet-9.pdf
repository: llm_database
chapter: Chapter 6. Power
section: 6.5.6. Programmer’s model
pages: 492-494
type: technical_spec
generated_at: 2026-03-01T11:44:31.548839+00:00
---

# 6.5.6. Programmer’s model

6.5.6. Programmer’s model

6.5.6.1. Sleep

The hello_sleep example (hello_sleep_aon.c in the pico-playground GitHub repository) demonstrates sleep mode. The

hello_sleep application (and underlying functions) takes the following steps:

1. Switches all clocks in the system to run from XOSC.

2. Configures an alarm in the AON Timer for 10 seconds in the future.

3. Sets the AON Timer clock as the only clock running in sleep mode using the SLEEP_ENx registers (see SLEEP_EN0).

4. Enables deep sleep in the processor.

5. Calls __wfi on processor, which will put the processor into deep sleep until woken by the AON Timer interrupt.

6. After 10 seconds, the AON Timer interrupt clears the alarm and then calls a user supplied callback function.

7. The callback function ends the example application.

NOTE

To enter sleep mode, you must enable deep sleep on both proc0 and proc1, call __wfi, and ensure the DMA is

stopped.

hello_sleep makes use of functions in pico_sleep of the Pico Extras. In particular, sleep_goto_sleep_until puts the

processor to sleep until woken up by an AON Timer time assumed to be in the future.

6.5. Power reduction strategies
491

RP2350 Datasheet

Pico Extras: https://github.com/raspberrypi/pico-extras/blob/master/src/rp2_common/pico_sleep/sleep.c Lines 159 - 183

```c
159 void sleep_goto_sleep_until(struct timespec *ts, aon_timer_alarm_handler_t callback)
160 {
161 
162     // We should have already called the sleep_run_from_dormant_source function
163     // This is only needed for dormancy although it saves power running from xosc while
    sleeping
164     //assert(dormant_source_valid(_dormant_source));
165 
166     clocks_hw->sleep_en0 = CLOCKS_SLEEP_EN0_CLK_REF_POWMAN_BITS;
167     clocks_hw->sleep_en1 = 0x0;
168 
169     aon_timer_enable_alarm(ts, callback, false);
170 
171     stdio_flush();
172 
173     // Enable deep sleep at the proc
174     processor_deep_sleep();
175 
176     // Go to sleep
177     __wfi();
178 }
```

6.5.6.2. DORMANT

The hello_dormant example, hello_dormant_gpio.c in the pico-playground GitHub repository, demonstrates the DORMANT

state. The example takes the following steps:

1. Switches all clocks in the system to run from XOSC.

2. Configures a GPIO interrupt for the dormant_wake hardware, which can wake both the ROSC and XOSC from dormant

mode.

3. Puts the XOSC into dormant mode, which stops all processor execution (and all other clocked logic on the chip)

immediately.

4. When GPIO 10 goes high, the XOSC restarts and program execution continues.

hello_dormant uses sleep_goto_dormant_until_pin under the hood:

Pico Extras: https://github.com/raspberrypi/pico-extras/blob/master/src/rp2_common/pico_sleep/sleep.c Lines 258 - 282

```c
258 void sleep_goto_dormant_until_pin(uint gpio_pin, bool edge, bool high) {
259     bool low = !high;
260     bool level = !edge;
261 
262     // Configure the appropriate IRQ at IO bank 0
263     assert(gpio_pin < NUM_BANK0_GPIOS);
264 
265     uint32_t event = 0;
266 
267     if (level && low) event = IO_BANK0_DORMANT_WAKE_INTE0_GPIO0_LEVEL_LOW_BITS;
268     if (level && high) event = IO_BANK0_DORMANT_WAKE_INTE0_GPIO0_LEVEL_HIGH_BITS;
269     if (edge && high) event = IO_BANK0_DORMANT_WAKE_INTE0_GPIO0_EDGE_HIGH_BITS;
270     if (edge && low) event = IO_BANK0_DORMANT_WAKE_INTE0_GPIO0_EDGE_LOW_BITS;
271 
272     gpio_init(gpio_pin);
273     gpio_set_input_enabled(gpio_pin, true);
274     gpio_set_dormant_irq_enabled(gpio_pin, event, true);
275 
```

6.5. Power reduction strategies
492

RP2350 Datasheet

```c
276     _go_dormant();
277     // Execution stops here until woken up
278 
279     // Clear the irq so we can go back to dormant mode again if we want
280     gpio_acknowledge_irq(gpio_pin, event);
281     gpio_set_input_enabled(gpio_pin, false);
282 }
```

6.5. Power reduction strategies
493
