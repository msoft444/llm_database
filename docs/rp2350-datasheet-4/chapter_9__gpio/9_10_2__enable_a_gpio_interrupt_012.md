---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 9. GPIO
section: 9.10.2. Enable a GPIO interrupt
pages: 604-604
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 9.10.2. Enable a GPIO interrupt

![Page 604 figure](images/fig_p0604.png)

9.10.2. Enable a GPIO interrupt

The SDK provides a method of being interrupted when a GPIO pin changes state:

SDK: https://github.com/raspberrypi/pico-sdk/blob/master/src/rp2_common/hardware_gpio/gpio.c Lines 186 - 196

186 void gpio_set_irq_enabled(uint gpio, uint32_t events, bool enabled) {

187     // either this call disables the interrupt or callback should already be set.

188     // this protects against enabling the interrupt without callback set

189     assert(!enabled || irq_has_handler(IO_IRQ_BANK0));

191     // Separate mask/force/status per-core, so check which core called, and

192     // set the relevant IRQ controls.

193     io_bank0_irq_ctrl_hw_t *irq_ctrl_base = get_core_num() ?

194                                       &io_bank0_hw->proc1_irq_ctrl : &io_bank0_hw-

195     _gpio_set_irq_enabled(gpio, events, enabled, irq_ctrl_base);

gpio_set_irq_enabled uses a lower level function _gpio_set_irq_enabled:

SDK: https://github.com/raspberrypi/pico-sdk/blob/master/src/rp2_common/hardware_gpio/gpio.c Lines 173 - 184

173 static void _gpio_set_irq_enabled(uint gpio, uint32_t events, bool enabled,

    io_bank0_irq_ctrl_hw_t *irq_ctrl_base) {

174     // Clear stale events which might cause immediate spurious handler entry

175     gpio_acknowledge_irq(gpio, events);

177     io_rw_32 *en_reg = &irq_ctrl_base->inte[gpio / 8];

178     events <<= 4 * (gpio % 8);

181         hw_set_bits(en_reg, events);

183         hw_clear_bits(en_reg, events);

The user provides a pointer to a callback function that is called when the GPIO event happens. An example application

that uses this system is hello_gpio_irq:

Pico Examples: https://github.com/raspberrypi/pico-examples/blob/master/gpio/hello_gpio_irq/hello_gpio_irq.c

 2  * Copyright (c) 2020 Raspberry Pi (Trading) Ltd.

 4  * SPDX-License-Identifier: BSD-3-Clause

9.10. Software examples
603
