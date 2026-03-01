---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.4.5. SDK access to the API
pages: 381-381
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 5.4.5. SDK access to the API

5.4.5. SDK access to the API

Bootrom functions are exposed in the SDK via the pico_bootrom library (see pico_bootrom).

Each bootrom function has a rom_ wrapper function that looks up the bootrom function address and calls it.

The SDK provides a simple implementation of exclusive access via bootrom_acquire_lock_blocking(n) and

bootrom_release_lock(n). When enabled, as it is by default (PICO_BOOTROM_LOCKING_ENABLED=1 is defined) the SDK enables

bootrom locking via LOCK_ENABLE, and these two functions use the other SHA_256/FLASH_OP/OTP boot locks to take ownership

of/release ownership of the corresponding bootrom resource.

The rom_ wrapper functions the SDK call bootrom_acquire_lock_locking and bootrom_relead_lock functions around bootrom

calls that have locking requirements.
