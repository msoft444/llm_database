---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 7. Resets
section: 7.5.3. List of Registers
pages: 506-508
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 7.5.3. List of Registers

![Page 506 figure](images/fig_p0506.png)

RP2350 Datasheet

155     // 0x00000004 [2]     DMA          (0)

156     // 0x00000002 [1]     BUSCTRL      (0)

157     // 0x00000001 [0]     ADC          (0)

158     io_ro_32 reset_done;

159 } resets_hw_t;

This struct defines the following registers:

• reset: This register contains a bit for each component that can be reset. When set to 1, the reset is asserted. If the

bit is cleared, the reset is deasserted.
• wdsel: This register contains a bit for each component that can be reset. When set to 1, this component will reset if

the watchdog fires. If you reset the power-on state machine, the entire reset controller will reset, which includes

every component.
• reset_done: This register contains a bit for each component that is automatically set when the component is out of

reset. This allows software to wait for this status bit in case the component requires initialisation before use.

The SDK defines reset functions as follows:

SDK: https://github.com/raspberrypi/pico-sdk/blob/master/src/rp2_common/hardware_resets/include/hardware/resets.h Lines 159 - 161

159 static __force_inline void reset_block(uint32_t bits) {

160     reset_block_mask(bits);

161 }

SDK: https://github.com/raspberrypi/pico-sdk/blob/master/src/rp2_common/hardware_resets/include/hardware/resets.h Lines 163 - 165

163 static __force_inline void unreset_block(uint32_t bits) {

164     unreset_block_mask(bits);

165 }

SDK: https://github.com/raspberrypi/pico-sdk/blob/master/src/rp2_common/hardware_resets/include/hardware/resets.h Lines 167 - 169

167 static __force_inline void unreset_block_wait(uint32_t bits) {

168     return unreset_block_mask_wait_blocking(bits);

169 }

One example use of reset functions is the UART driver, which defines a uart_reset function that selects a different bit of

the reset register depending on the UART specified:

SDK: https://github.com/raspberrypi/pico-sdk/blob/master/src/rp2_common/hardware_uart/uart.c Lines 32 - 38

32 static inline void uart_reset(uart_inst_t *uart) {

33     reset_block_num(uart_get_reset_num(uart));

34 }

35 

36 static inline void uart_unreset(uart_inst_t *uart) {

37     unreset_block_num_wait_blocking(uart_get_reset_num(uart));

38 }

7.5.3. List of Registers

The reset controller registers start at a base address of 0x40020000 (defined as RESETS_BASE in SDK).

7.5. Subsystem resets
505

![Page 507 figure](images/fig_p0507.png)

RP2350 Datasheet

| Offset | Name | Info |
| --- | --- | --- |
| 0x0 | RESET |  |
| 0x4 | WDSEL |  |
| 0x8 | RESET_DONE |  |

Table 534. List of

RESETS: RESET Register

Offset: 0x0

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:29 | Reserved. | - | - |
| 28 | USBCTRL | RW | 0x1 |
| 27 | UART1 | RW | 0x1 |
| 26 | UART0 | RW | 0x1 |
| 25 | TRNG | RW | 0x1 |
| 24 | TIMER1 | RW | 0x1 |
| 23 | TIMER0 | RW | 0x1 |
| 22 | TBMAN | RW | 0x1 |
| 21 | SYSINFO | RW | 0x1 |
| 20 | SYSCFG | RW | 0x1 |
| 19 | SPI1 | RW | 0x1 |
| 18 | SPI0 | RW | 0x1 |
| 17 | SHA256 | RW | 0x1 |
| 16 | PWM | RW | 0x1 |
| 15 | PLL_USB | RW | 0x1 |
| 14 | PLL_SYS | RW | 0x1 |
| 13 | PIO2 | RW | 0x1 |
| 12 | PIO1 | RW | 0x1 |
| 11 | PIO0 | RW | 0x1 |
| 10 | PADS_QSPI | RW | 0x1 |
| 9 | PADS_BANK0 | RW | 0x1 |
| 8 | JTAG | RW | 0x1 |
| 7 | IO_QSPI | RW | 0x1 |
| 6 | IO_BANK0 | RW | 0x1 |
| 5 | I2C1 | RW | 0x1 |
| 4 | I2C0 | RW | 0x1 |
| 3 | HSTX | RW | 0x1 |
| 2 | DMA | RW | 0x1 |
| 1 | BUSCTRL | RW | 0x1 |

Table 535. RESET

7.5. Subsystem resets
506

![Page 508 figure](images/fig_p0508.png)

RP2350 Datasheet

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 0 | ADC | RW | 0x1 |

RESETS: WDSEL Register

Offset: 0x4

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:29 | Reserved. | - | - |
| 28 | USBCTRL | RW | 0x0 |
| 27 | UART1 | RW | 0x0 |
| 26 | UART0 | RW | 0x0 |
| 25 | TRNG | RW | 0x0 |
| 24 | TIMER1 | RW | 0x0 |
| 23 | TIMER0 | RW | 0x0 |
| 22 | TBMAN | RW | 0x0 |
| 21 | SYSINFO | RW | 0x0 |
| 20 | SYSCFG | RW | 0x0 |
| 19 | SPI1 | RW | 0x0 |
| 18 | SPI0 | RW | 0x0 |
| 17 | SHA256 | RW | 0x0 |
| 16 | PWM | RW | 0x0 |
| 15 | PLL_USB | RW | 0x0 |
| 14 | PLL_SYS | RW | 0x0 |
| 13 | PIO2 | RW | 0x0 |
| 12 | PIO1 | RW | 0x0 |
| 11 | PIO0 | RW | 0x0 |
| 10 | PADS_QSPI | RW | 0x0 |
| 9 | PADS_BANK0 | RW | 0x0 |
| 8 | JTAG | RW | 0x0 |
| 7 | IO_QSPI | RW | 0x0 |
| 6 | IO_BANK0 | RW | 0x0 |
| 5 | I2C1 | RW | 0x0 |
| 4 | I2C0 | RW | 0x0 |
| 3 | HSTX | RW | 0x0 |
| 2 | DMA | RW | 0x0 |
| 1 | BUSCTRL | RW | 0x0 |
| 0 | ADC | RW | 0x0 |

Table 536. WDSEL

RESETS: RESET_DONE Register

7.5. Subsystem resets
507
