---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.2.13. Bus clear feature
pages: 1003-1003
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 12.2.13. Bus clear feature

Recovery Clocks
Spike length counter
SCL
Internal filtered SCL
0
1
2
3
0
1
2
3
4
5
0
Figure 87. Spike
Suppression Example
NOTE
There is a 2-stage synchronizer on the SCL input. For the sake of simplicity, this synchronization delay was not
included in the timing diagram in Figure 87.
The I2C Bus Specification calls for different maximum spike lengths according to the operating mode (50 ns for SS and
FS). Register IC_FS_SPKLEN holds the maximum spike length for SS and FS modes.
This register is 8 bits wide and accessible through the APB interface for reads and writes. However, you can only write
to this register when the DW_apb_i2c is disabled. The minimum value that can be programmed into these registers is one;
attempting to program a value smaller than one results in the value one being written.
The default value for these registers is based on the value of 100 ns for ic_clk period, so should be updated for the
clk_sys period in use on RP2350.
NOTE
• Because the minimum value that can be programmed into the IC_FS_SPKLEN register is one, the spike length
specification can be exceeded for low frequencies of ic_clk. Consider the simple example of a 10 MHz (100 ns
period) ic_clk; in this case, the minimum spike length that can be programmed is 100 ns, which means that
spikes up to this length are suppressed.
• Standard synchronization logic (two flip-flops in series) is implemented upstream of the spike suppression
logic and is not affected in any way by the contents of the spike length registers or the operation of the spike
suppression logic; the two operations (synchronization and spike suppression) are completely independent.
Because the SCL and SDA inputs are asynchronous to ic_clk, there is one ic_clk cycle uncertainty in the sampling
of these signals. Depending on when they occur relative to the rising edge of ic_clk, spikes of the same original
length might show a difference of one ic_clk cycle after being sampled.
• Spike suppression is symmetrical; the behaviour is exactly the same for transitions from zero to one and from
one to zero.
12.2.12. Fast mode plus operation
In fast mode plus, the DW_apb_i2c extends fast mode operation to be support speeds up to 1000 kb/s. To enable the
DW_apb_i2c for fast mode plus operation, perform the following steps before initiating any data transfer:
1. Set ic_clk frequency greater than or equal to 32 MHz (refer to Section 12.2.14.2.1).
2. Program the IC_CON register [2:1] = 2’b10 for fast mode or fast mode plus.
3. Program IC_FS_SCL_LCNT and IC_FS_SCL_HCNT registers to meet the fast mode plus SCL (refer to Section
12.2.14).
4. Program the IC_FS_SPKLEN register to suppress the maximum spike of 50 ns.
5. Program the IC_SDA_SETUP register to meet the minimum data setup time (tSU; DAT).
12.2.13. Bus clear feature
DW_apb_i2c supports the bus clear feature that provides graceful recovery of data SDA and clock SCL lines during unlikely
events in which either the clock or data line is stuck at LOW.
RP2350 Datasheet
12.2. I2C
1002

