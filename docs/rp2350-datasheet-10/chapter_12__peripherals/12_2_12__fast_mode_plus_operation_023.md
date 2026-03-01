---
source_pdf: rp2350-datasheet-10.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.2.12. Fast mode plus operation
pages: 1003-1003
type: technical_spec
generated_at: 2026-03-01T12:20:14.494106+00:00
---

# 12.2.12. Fast mode plus operation

12.2.12. Fast mode plus operation

In fast mode plus, the DW_apb_i2c extends fast mode operation to be support speeds up to 1000 kb/s. To enable the

DW_apb_i2c for fast mode plus operation, perform the following steps before initiating any data transfer:

1. Set ic_clk frequency greater than or equal to 32 MHz (refer to Section 12.2.14.2.1).

2. Program the IC_CON register [2:1] = 2’b10 for fast mode or fast mode plus.

3. Program IC_FS_SCL_LCNT and IC_FS_SCL_HCNT registers to meet the fast mode plus SCL (refer to Section

12.2.14).

4. Program the IC_FS_SPKLEN register to suppress the maximum spike of 50 ns.

5. Program the IC_SDA_SETUP register to meet the minimum data setup time (tSU; DAT).
