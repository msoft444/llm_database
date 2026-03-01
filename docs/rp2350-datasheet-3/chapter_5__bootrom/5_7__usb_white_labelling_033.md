---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.7. USB white-labelling
pages: 413-413
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 5.7. USB white-labelling

![Page 413 figure](images/fig_p0413.png)

RP2350 Datasheet

Offset
Name
Description

0x04
dStatusCode
OK (0)
The command completed successfully (or is in still in

progress)

UNKNOWN_CMD (1)
The ID of the command was unrecognised

INVALID_CMD_LENGTH (2)
The length of the command request was incorrect

INVALID_TRANSFER_LENGTH (3)
The data transfer length was incorrect given the

command

INVALID_ADDRESS (4)
The address specified was invalid for the command type;

this means that the address didn’t match the type (flash or

RAM) that the command was expecting

BAD_ALIGNMENT (5)
The address specified was incorrectly aligned according

to the requirements of the command

INTERLEAVED_WRITE (6)
A Mass Storage Interface UF2 write has interfered with the

current operation. The command was abandoned with

unknown status. This doesn’t happen if you have exclusive

access.

REBOOTING (7)
The device is in the process of rebooting, so the command

has been ignored.

UNKNOWN_ERROR (8)
Some non-specific error occurred.

INVALID_STATE (9)
Something happened or failed to happen in the past, and

consequently the request can’t (currently) be serviced.

NOT_PERMITTED (10)
Permission violation, such as write to read-only flash

partition.

INVALID_ARG (11)
Argument is outside of range of supported values.

BUFFER_TOO_SMALL (12)
The provided buffer was too small to hold the result.

PRECONDITION_NOT_MET (13)
The operation failed because another bootrom function

must be called first.

MODIFIED_DATA (14)
Cached data was determined to be inconsistent with the

full version of the data it was calculated from.

INVALID_DATA (15)
A data structure failed to validate.

NOT_FOUND (16)
Attempted to access something that doesn’t exist; or a

search failed.

UNSUPPORTED_MODIFICATION (17) Write is impossible based on previous writes, such as

attempting to clear an OTP bit.

0x08
bCmdId
The ID of the command

0x09
bInProgress
1 if the command is still in

0 otherwise

progress

0x0a
reserved
(6 zero bytes)

5.7. USB white-labelling

To brand RP2350-based products, customers may replace identifying information exposed by USB interfaces. We call

this white-labelling, and you can accomplish it in RP2350 by specifying values in OTP.

5.7. USB white-labelling
412
