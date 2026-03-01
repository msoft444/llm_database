---
source_pdf: rp2350-datasheet-10.pdf
repository: llm_database
chapter: Chapter 13. OTP
section: 13.5.3. Lock encoding in OTP
pages: 1277-1277
type: technical_spec
generated_at: 2026-03-01T12:20:14.494106+00:00
---

# 13.5.3. Lock encoding in OTP

13.5.3. Lock encoding in OTP

Page locks are encoded as a 16-bit value. This value is stored as a pair of triple-redundant bytes, each byte occupying a

24-bit OTP row.

The lock halfword is encoded as follows:

| Bits | Purpose |
| --- | --- |
| 2:0 | Write key index, or 0 if no write key |
| 5:3 | Read key index, or 0 if no read key |
| 6 | No-key state, 0=Read-only 1=Inaccessible |
| 7 | Reserved |
| 9:8 | Secure lock state (thermometer code 0 → 2) |
| 11:10 | Non-secure lock state (thermometer code 0 → 2) |
| 13:12 | PicoBoot lock state (thermometer code 0 → 2) or software-defined use if PicoBoot OTP is disabled |
| 15:12 | Reserved |
