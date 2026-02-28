---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.4.4. API functions and exclusive access
pages: 380-380
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 5.4.4. API functions and exclusive access

![Page 380 figure](images/fig_p0380.png)

RP2350 Datasheet

| Name | Value | Description |
| --- | --- | --- |
| BOOTROM ERROR INVALID ADDRESS
_ _ _ | -10 | An address argument was out-of-bounds or was
determined to be an address that the caller may not
access. |
| BOOTROM ERROR BAD ALIGNMENT
_ _ _ | -11 | An address passed to the function was not correctly
aligned. |
| BOOTROM ERROR INVALID STATE
_ _ _ | -12 | Something happened or failed to happen in the past, and
consequently the request cannot currently be serviced. |
| BOOTROM ERROR BUFFER TOO SMALL
_ _ _ _ | -13 | A user-allocated buffer was too small to hold the result or
working state of the function. |
| BOOTROM ERROR PRECONDITION NOT MET
_ _ _ _ | -14 | The call failed because another bootrom function must be
called first. |
| BOOTROM ERROR MODIFIED DATA
_ _ _ | -15 | Cached data was determined to be inconsistent with the
full version of the data it was copied from. |
| BOOTROM ERROR INVALID DATA
_ _ _ | -16 | The contents of a data structure are invalid |
| BOOTROM ERROR NOT FOUND
_ _ _ | -17 | An attempt was made to access something that does not
exist; or, a search failed. |
| BOOTROM ERROR UNSUPPORTED MODIFICATION
_ _ _ | -18 | Modification is impossible based on current state. This
might occur, for example, when attempting to clear an
OTP bit. |
| BOOTROM ERROR LOCK REQUIRED
_ _ _ | -19 | A required lock is not owned. See Section 5.4.4. |

5.4.4. API functions and exclusive access

Various bootrom functions require access to parts of the system which:

• cannot be safely accessed by both cores at once, or
• limit the functionality of other hardware when in use

For example:

• Programming OTP: it is not possible to read from the memory mapped OTP data regions at the same time as

accessing its serial programming interface.
• Use of the SHA-256 block: only one SHA-256 sum can be in progress at a time.
• Using the QSPI direct-mode interface to program the flash causes XIP access to return a bus fault.

It is beyond the purview of the bootrom to implement a locking strategy, as the style and scope of the locking required

is entirely up to how the application itself uses these resources.

Nevertheless, it is important that, say, a Non-secure call to a flash programming API can’t cause a hard fault in other

Secure code running from flash. There must be some way for user software to coordinate with bootrom APIs on such

changes of state. The bootrom implements the mechanism but not the policy for mutual exclusion over bootrom API

calls.

The solution the bootrom provides is to use the boot locks (boot RAM registers BOOTLOCK0 through BOOTLOCK7) to

inform the bootrom which resources are currently owned by the caller and therefore safe for it to use.

To enable lock checking in bootrom APIs, set boot lock 7 (LOCK_ENABLE) to the claimed state. When enabled, bootrom

functions which use certain hardware resources (listed below) will check the status of the boot lock assigned to that

resource, and return BOOTROM_ERROR_LOCK_REQUIRED if that lock is not in the claimed state.

Before calling a bootrom function with locking enabled, you must claim the relevant locks. It may take multiple attempts

5.4. Bootrom APIs
379
