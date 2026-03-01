---
source_pdf: rp2350-datasheet-5.pdf
repository: llm_database
chapter: Chapter 13. OTP
section: 13.5.1. Lock progression
pages: 1275-1276
type: technical_spec
generated_at: 2026-03-01T04:13:03.328193+00:00
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

RP2350 Datasheet

• The lock registers ignore writes of lower-than-current values.

Secure and Non-secure use separate lock values, which can advance independently of one another. There is no

hardware distinction between Non-secure Read/Write and Non-secure Read-only, since Non-secure can not directly

write to the OTP anyway. It is still worth encoding, because Secure software performing a write on Non-secure

software’s behalf can check and enforce the Non-secure write lock.

You can reprogram bits from any state to any higher state. Locks use a 2-bit thermometer code: the initial all-zeroes

state is read-write, and locks are advanced by programming first bit 0, then bit 1.

Lock bits in OTP are triple-redundant with a majority vote. They can’t be ECC-protected, because they may be mutated

bit-by-bit over multiple programming operations.

The OTP-resident lock bits are write-protected by their own Secure lock level. The lock pages are always world-readable.

The Secure lock registers can be advanced by Secure code, and are world-readable.

The Non-secure lock registers can be advanced by Secure or Non-secure code, and are world-readable.
