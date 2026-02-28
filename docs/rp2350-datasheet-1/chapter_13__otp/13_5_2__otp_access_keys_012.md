---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 13. OTP
section: 13.5.2. OTP access keys
pages: 1276-1276
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 13.5.2. OTP access keys

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
13.5.2. OTP access keys
Page 61 contains 128-bit keys. Each key has a valid bit: when set, the key becomes completely inaccessible to
software. The keys are always read out into hidden registers by hardware during startup so that hardware can perform
key comparisons without exposing the keys to software.
Pages can require specific keys for some page permissions. To unlock the page, the user writes their key to a write-only
register in the OTP block. The page remains unlocked for as long as the correct key is present in this register. To re-lock
the page, erase the active key by writing zeroes to the key register.
The per-page lock config specifies the following:
• a read key index 1-7, or 0 if there is no read key
• a write key index 1-7 or 0 if there is no write key
• the no-key state: either Read-only or Inaccessible, state of the page when no registered key has been entered by
software into the key register
The no-key state is encoded as follows:
• 0 for Read-only (lock level 1)
• 1 for Inaccessible (lock level 2).

TIP
Key index 7 does not exist in the configuration. If you specify key index 7, it is guaranteed to never match.
The hardware determines the key lock level by comparing the entered key to the key config of the current page, as
follows:
1. If no keys are registered, the key lock level is 0
2. Else if keys are registered and no matching key is entered, the key lock level is 2 or 1 depending on the "no-key
state" config
3. Else if a write key is registered and present, the key lock level is 0
4. Else if a read key is registered and present, the key lock level is 1
Hardware compares the key lock level to the page’s lock level for the current security domain (Secure/Non-secure) and
takes whichever is higher. For example, if a page has been made Non-secure read-only, there is nothing a key can do to
make it Non-secure writable.
There are six 128-bit access keys stored in the OTP. Keys 5 and 6 also function as the Secure debug access key and
RP2350 Datasheet
13.5. Page locks
1275

