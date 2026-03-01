---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 13. OTP
section: 13.3.1. Lock shim
pages: 1271-1271
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 13.3.1. Lock shim

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

