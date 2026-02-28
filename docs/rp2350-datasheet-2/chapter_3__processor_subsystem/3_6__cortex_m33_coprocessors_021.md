---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.6. Cortex-M33 coprocessors
pages: 101-101
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 3.6. Cortex-M33 coprocessors

![Page 101 figure](images/fig_p0101.png)

RP2350 Datasheet

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 5 | DBG_POW_RESET: When DBG_POW_OVRD_RESET=1 this register bit controls
the resets for all domains. 1 = reset. 0 = not reset. | RW | 0x0 |
| 4 | DBG_POW_OVRD_RESET: Enables DBG_POW_RESET to control the resets for
the power manager and the switched-core. Essentially that is everythjing
except the Coresight 2-wire interface and the RP_AP registers. | RW | 0x0 |
| 3 | DBG_POW_ISO: When DBG_POW_OVRD_ISO=1 this register bit controls the
isolation gates for all domains. 1 = isolated. 0 = not isolated. | RW | 0x0 |
| 2 | DBG_POW_OVRD_ISO: Enables DBG_POW_ISO to control the isolation gates
between domains. | RW | 0x0 |
| 1 | DBG_POW_OVRD_LARGE_REQ: Turn on the large power switches for all
domains. This should not be done until sufficient time has been allowed for
the small switches to bring the supplies up. Switching the large switches on
too soon risks browning out the always-on domain and corrupting these very
registers. | RW | 0x0 |
| 0 | DBG_POW_OVRD_SMALL_REQ: Turn on the small power switches for all
domains. This switches on chain 0 for each domain and switches off chains 2
& 3 and the large power switch chain. This will bring the power up for all
domains without browning out the always-on power domain. | RW | 0x0 |

RP_AP: DBG_POW_OUTPUT_TO_GPIO Register

Offset: 0x01c

Description

Send some, or all, bits of DBG_POW_STATE_SWCORE to gpios.

Bit 0 sends bit 0 of DBG_POW_STATE_SWCORE to GPIO 34

Bit 1 sends bit 1 of DBG_POW_STATE_SWCORE to GPIO 35

Bit 2 sends bit 2 of DBG_POW_STATE_SWCORE to GPIO 36

1. +

2. + Bit 11 sends bit 11 of DBG_POW_STATE_SWCORE to GPIO 45

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:12 | Reserved. | - | - |
| 11:0 | ENABLE | RW | 0x000 |

Table 107.

DBG_POW_OUTPUT_T

O_GPIO Register

RP_AP: IDR Register

Offset: 0xdfc

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:0 | Standard Coresight ID Register | RO | - |

Table 108. IDR

3.6. Cortex-M33 coprocessors

The Cortex-M33 features a coprocessor port which transfers up to 64 bits per cycle between the processor and certain

closely-coupled hardware. The Cortex-M33’s built-in floating-point unit is an example of such a coprocessor, but

RP2350 adds three device-specific coprocessors to this interface. The following sections document these

coprocessors.

3.6. Cortex-M33 coprocessors
100
