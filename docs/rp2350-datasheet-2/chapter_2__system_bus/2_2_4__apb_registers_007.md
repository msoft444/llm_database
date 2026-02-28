---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 2. System bus
section: 2.2.4. APB registers
pages: 32-33
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 2.2.4. APB registers

![Page 32 figure](images/fig_p0032.png)

RP2350 Datasheet

| Bus Endpoint | Base Address |
| --- | --- |
| XIP_BASE | 0x10000000 |
| XIP_NOCACHE_NOALLOC_BASE | 0x14000000 |
| XIP_MAINTENANCE_BASE | 0x18000000 |
| XIP_NOCACHE_NOALLOC_NOTRANSLATE_BASE | 0x1c000000 |

Table 10. Address

map for XIP bus

segment

NOTE

XIP_SRAM_BASE no longer exists as a separate address range. Cache-as-SRAM is now achieved by pinning cache lines

within the cached XIP address space.

2.2.3. SRAM

SRAM is accessible to DMA, processor load/store, and processor instruction fetch.

SRAM0-3 and SRAM4-7 are always striped on bits 3:2 of the address:

| Bus Endpoint | Base Address |
| --- | --- |
| SRAM_BASE | 0x20000000 |
| SRAM_STRIPED_BASE | 0x20000000 |
| SRAM0_BASE | 0x20000000 |
| SRAM4_BASE | 0x20040000 |
| SRAM_STRIPED_END | 0x20080000 |

Table 11. Address

map for SRAM bus

segment, SRAM0-7

(striped)

There are two striped regions, each 256 kB in size, and each striped over 4 SRAM banks. SRAM0-3 are in the SRAM0

power domain, and SRAM4-7 are in the SRAM1 power domain.

SRAM 8-9 are always non-striped:

| Bus Endpoint | Base Address |
| --- | --- |
| SRAM8_BASE | 0x20080000 |
| SRAM9_BASE | 0x20081000 |
| SRAM_END | 0x20082000 |

Table 12. Address

map for SRAM bus

segment, SRAM8-9

(non-striped)

These smaller blocks of SRAM are useful for hoisting high-bandwidth data structures like the processor stacks. They

are in the SRAM1 power domain.

2.2.4. APB registers

APB peripheral registers are accessible to processor load/store and DMA only. Instruction fetch will always fail.

The APB peripheral segment provides access to control and configuration registers, as well as data access for lower-

bandwidth peripherals. APB writes cost a minimum of four cycles, and APB reads a minimum of three.

| Bus Endpoint | Base Address |
| --- | --- |
| SYSINFO_BASE | 0x40000000 |
| SYSCFG_BASE | 0x40008000 |

Table 13. Address

map for APB bus

segment

2.2. Address map
31

![Page 33 figure](images/fig_p0033.png)

RP2350 Datasheet

| Bus Endpoint | Base Address |
| --- | --- |
| CLOCKS_BASE | 0x40010000 |
| PSM_BASE | 0x40018000 |
| RESETS_BASE | 0x40020000 |
| IO_BANK0_BASE | 0x40028000 |
| IO_QSPI_BASE | 0x40030000 |
| PADS_BANK0_BASE | 0x40038000 |
| PADS_QSPI_BASE | 0x40040000 |
| XOSC_BASE | 0x40048000 |
| PLL_SYS_BASE | 0x40050000 |
| PLL_USB_BASE | 0x40058000 |
| ACCESSCTRL_BASE | 0x40060000 |
| BUSCTRL_BASE | 0x40068000 |
| UART0_BASE | 0x40070000 |
| UART1_BASE | 0x40078000 |
| SPI0_BASE | 0x40080000 |
| SPI1_BASE | 0x40088000 |
| I2C0_BASE | 0x40090000 |
| I2C1_BASE | 0x40098000 |
| ADC_BASE | 0x400a0000 |
| PWM_BASE | 0x400a8000 |
| TIMER0_BASE | 0x400b0000 |
| TIMER1_BASE | 0x400b8000 |
| HSTX_CTRL_BASE | 0x400c0000 |
| XIP_CTRL_BASE | 0x400c8000 |
| XIP_QMI_BASE | 0x400d0000 |
| WATCHDOG_BASE | 0x400d8000 |
| BOOTRAM_BASE | 0x400e0000 |
| ROSC_BASE | 0x400e8000 |
| TRNG_BASE | 0x400f0000 |
| SHA256_BASE | 0x400f8000 |
| POWMAN_BASE | 0x40100000 |
| TICKS_BASE | 0x40108000 |
| OTP_BASE | 0x40120000 |
| OTP_DATA_BASE | 0x40130000 |
| OTP_DATA_RAW_BASE | 0x40134000 |
| OTP_DATA_GUARDED_BASE | 0x40138000 |

2.2. Address map
32
