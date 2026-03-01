---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 13. OTP
section: 13.5.1. Lock progression
pages: 1275-1275
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 13.5.1. Lock progression

13.5.1. Lock progression

Due to hardware constraints (Section 13.3.1), read and write restrictions are not orthogonal: it’s impossible to disallow

reads to an address without also disallowing writes. So, the progression of locking for a given page is:

0. Read/Write

1. Read-only

2. Inaccessible

Lock state only increases. This is enforced in two ways:

• Due to the nature of OTP and the choice of encoding, you cannot lower the OTP values preloaded to the lock

registers during boot.

13.5. Page locks
1274
