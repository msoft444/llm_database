---
source_pdf: rp2350-datasheet-6.pdf
repository: llm_database
chapter: Chapter 13. OTP
section: 13.2. Background: OTP IP details
pages: 1270-1271
type: technical_spec
generated_at: 2026-03-01T05:39:21.783275+00:00
---

# 13.2. Background: OTP IP details

13.2. Background: OTP IP details

The RP2350 OTP subsystem uses the Synopsys NVM OTP IP, which comes in 3 parts:

• Integrated Power Supply (IPS), including:

◦Charge Pump (for programming)

◦Regulator (for reading)
• OTP Macro (SHF, Fuse)

◦4096 × 24 (8 kB with ECC, 16-bit ECC write granularity)
• Access port (AP), providing:

13.2. Background: OTP IP details
1269

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

## Embedded Images

![img_p1271_00.png](images/img_p1271_00.png)

