---
source_pdf: rp2350-datasheet-7.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.1.4. Hardware spinlocks
pages: 42-43
type: technical_spec
generated_at: 2026-03-01T06:07:53.318745+00:00
---

# 3.1.4. Hardware spinlocks

3.1.4. Hardware spinlocks

The SIO provides 32 hardware spinlocks, which can be used to manage mutually-exclusive access to shared software

resources. Each spinlock is a one-bit flag, mapped to a different address (from SPINLOCK0 to SPINLOCK31). Software

interacts with each spinlock with one of the following operations:

• Read: Attempt to claim the lock. Read value is non-zero if the lock was successfully claimed, or zero if the lock had

already been claimed by a previous read.
• Write (any value): Release the lock. The next attempt to claim the lock will succeed.

If both cores try to claim the same lock on the same clock cycle, core 0 succeeds.

Generally software will acquire a lock by repeatedly polling the lock bit ("spinning" on the lock) until it is successfully

claimed. This is inefficient if the lock is held for long periods, so generally the spinlocks should be used to protect short

critical sections of higher-level primitives such as mutexes, semaphores and queues.

For debugging purposes, the current state of all 32 spinlocks can be observed via SPINLOCK_ST.

NOTE

RP2350 has separate spinlocks for Secure and Non-secure SIO banks because sharing these registers would allow

Non-secure code to deliberately starve Secure code that attempts to acquire a lock. See Section 3.1.1.

NOTE

The processors on RP2350 support standard atomic/exclusive access instructions which, in concert with the global

exclusive monitor (Section 2.1.6), allow both cores to safely share variables in SRAM. The SIO spinlocks are still

included for compatibility with RP2040.

3.1. SIO
41

RP2350 Datasheet

NOTE

Due to RP2350-E2, writes to new SIO registers above an offset of +0x180 alias the spinlocks, causing spurious lock

releases. The SDK by default uses atomic memory accesses to implement the hardware_sync_spin_lock API, as a

workaround on RP2350 A2.
