---
source_pdf: rp2350-datasheet-9.pdf
repository: llm_database
chapter: Chapter 13. OTP
section: 13.5. Page locks
pages: 1275-1275
type: technical_spec
generated_at: 2026-03-01T11:44:31.548839+00:00
---

# 13.5. Page locks

13.5. Page locks

The OTP protection hardware logically segments OTP into 64 pages (0 through 63), each 128 bytes in size, or

equivalently 64 OTP rows.

Each page has a set of lock registers which determine read and write access for that page from Secure and Non-secure

code. The lock registers are preloaded from OTP at reset, and can then be advanced (i.e. made less permissive) by

software. Lock registers themselves are always world-readable.

Pages 61 through 63 are not so neatly described by a single set of lock registers. These pages store lock initialisation

metadata. For more details, see Section 13.5.4. This section describes the more common case of a page protected by a

set of page locks.
