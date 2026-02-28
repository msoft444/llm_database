---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 13. OTP
section: 13.3.1. Lock shim
pages: 1271-1271
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 13.3.1. Lock shim

RP2350 Datasheet

◦Basic read access

◦Programming access

◦ECC and bit redundancy

◦BOOT function, which polls for stable OTP power supply at start-of-day

13.3. Background: OTP hardware architecture

This diagram shows the integration of the three Synopsys IP components, and the Raspberry Pi hardware added to

make this all function in the context of RP2350’s system and security architecture. More specifically:

• APB interface(s) to connect to the SoC
• Internal ring oscillator with clock edge randomisation
• Power-up state machine, running off the ring oscillator
• Lock shim, sitting between the SNPS RTL and the memory core (fuse)

Figure 142. OTP

architecture

The OTP subsystem clock is initially provided by the OTP boot oscillator (Section 13.3.3) during hardware startup, but

switches to clk_ref before any software runs on the processors. The frequency of clk_ref must not exceed 25 MHz

when accessing the OTP.

13.3.1. Lock shim

The lock shim is inserted between the Synopsys AP block and the SHF block, and is used to enforce read/write page

locks, based on:

• The OTP address presented on the AP → SHF bus
• The read/write strobe on the AP → SHF bus
• The security attribute of the upstream bus access which caused this SHF access (assumed to be Secure if SBPI is

currently enabled via USR.DCTRL)

Because the Synopsys AP performs both reads and writes in the course of programming an OTP row, it is impossible to

disable reads to an address without also disabling writes. Three lock states are supported:

13.3. Background: OTP hardware architecture
1270

## Embedded Images

![img_p1271_00.png](images/img_p1271_00.png)

