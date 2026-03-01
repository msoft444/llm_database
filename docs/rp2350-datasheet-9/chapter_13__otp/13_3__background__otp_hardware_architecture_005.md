---
source_pdf: rp2350-datasheet-9.pdf
repository: llm_database
chapter: Chapter 13. OTP
section: 13.3. Background: OTP hardware architecture
pages: 1271-1271
type: technical_spec
generated_at: 2026-03-01T11:44:31.548839+00:00
---

# 13.3. Background: OTP hardware architecture

13.3. Background: OTP hardware architecture

This diagram shows the integration of the three Synopsys IP components, and the Raspberry Pi hardware added to

make this all function in the context of RP2350’s system and security architecture. More specifically:

• APB interface(s) to connect to the SoC
• Internal ring oscillator with clock edge randomisation
• Power-up state machine, running off the ring oscillator
• Lock shim, sitting between the SNPS RTL and the memory core (fuse)

*Figure 142. OTP architecture*

The OTP subsystem clock is initially provided by the OTP boot oscillator (Section 13.3.3) during hardware startup, but

switches to clk_ref before any software runs on the processors. The frequency of clk_ref must not exceed 25 MHz

when accessing the OTP.

## Embedded Images

![img_p1271_00.png](images/img_p1271_00.png)

