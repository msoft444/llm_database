---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.2.11. Spike suppression
pages: 1002-1002
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 12.2.11. Spike suppression

12.2.11. Spike suppression

The DW_apb_i2c contains programmable spike suppression logic that matches requirements imposed by the I2C Bus

Specification for SS/FS modes. This logic is based on counters that monitor the input signals (SCL and SDA), checking if

they remain stable for a predetermined amount of ic_clk cycles before they are sampled internally. There is one

separate counter for each signal (SCL and SDA). The number of ic_clk cycles can be programmed by the user. The value

should account for the frequency of ic_clk and the relevant spike length specification. Each counter starts whenever its

input signal changes value. Depending on the behaviour of the input signal, one of the following scenarios occurs:

• The input signal remains unchanged until the counter reaches its count limit value. When this happens, the counter

resets and stops, and the internal version of the signal updates to the input value.
• The input signal changes again before the counter reaches its count limit value. When this happens, the counter

resets and stops, but the internal version of the signal does not update.

The timing diagram in Figure 87 illustrates the behaviour described above.

12.2. I2C
1001
