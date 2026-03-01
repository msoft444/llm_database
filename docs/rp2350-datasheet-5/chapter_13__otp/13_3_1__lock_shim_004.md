---
source_pdf: rp2350-datasheet-5.pdf
repository: llm_database
chapter: Chapter 13. OTP
section: 13.3.1. Lock shim
pages: 1271-1272
type: technical_spec
generated_at: 2026-03-01T04:13:03.328193+00:00
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

RP2350 Datasheet

• Read/Write
• Read-only
• Inaccessible

The full locking scheme is described in in Section 13.5, but to summarise:

• The lock state of each OTP page is read from OTP at boot time.
• There is a separate copy of the lock state for Secure/Non-secure accesses. The lock shim applies the Secure read

permissions to Secure reads, and the Non-secure read permissions to Non-secure reads. There is no such rule for

writes, because Non-secure code is not capable of accessing the programming hardware.
• The lock encoding in OTP storage is such that a page can always be locked down to a less permissive state (in the

order RW → RO → Inaccessible) but can never return to a more permissive state.
• Software can advance the state of each individual lock at runtime without programming OTP, and this lasts until

the OTP PSM is re-run.
• Software locks also obey the lock progression order (RW → RO → Inaccessible) and can not be regressed.

The full locking scheme is described in in Section 13.5.

## Embedded Images

![img_p1271_00.png](images/img_p1271_00.png)

