---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.4.4. API functions and exclusive access
pages: 380-380
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 5.4.4. API functions and exclusive access

Name
Value
Description
BOOTROM_ERROR_INVALID_ADDRESS
-10
An address argument was out-of-bounds or was
determined to be an address that the caller may not
access.
BOOTROM_ERROR_BAD_ALIGNMENT
-11
An address passed to the function was not correctly
aligned.
BOOTROM_ERROR_INVALID_STATE
-12
Something happened or failed to happen in the past, and
consequently the request cannot currently be serviced.
BOOTROM_ERROR_BUFFER_TOO_SMALL
-13
A user-allocated buffer was too small to hold the result or
working state of the function.
BOOTROM_ERROR_PRECONDITION_NOT_MET
-14
The call failed because another bootrom function must be
called first.
BOOTROM_ERROR_MODIFIED_DATA
-15
Cached data was determined to be inconsistent with the
full version of the data it was copied from.
BOOTROM_ERROR_INVALID_DATA
-16
The contents of a data structure are invalid
BOOTROM_ERROR_NOT_FOUND
-17
An attempt was made to access something that does not
exist; or, a search failed.
BOOTROM_ERROR_UNSUPPORTED_MODIFICATION
-18
Modification is impossible based on current state. This
might occur, for example, when attempting to clear an
OTP bit.
BOOTROM_ERROR_LOCK_REQUIRED
-19
A required lock is not owned. See Section 5.4.4.
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
RP2350 Datasheet
5.4. Bootrom APIs
379

