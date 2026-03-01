---
source_pdf: rp2350-datasheet-9.pdf
repository: llm_database
chapter: Chapter 13. OTP
section: 13.2. Background: OTP IP details
pages: 1270-1271
type: technical_spec
generated_at: 2026-03-01T11:44:31.548839+00:00
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

## Embedded Images

![img_p1271_00.png](images/img_p1271_00.png)

