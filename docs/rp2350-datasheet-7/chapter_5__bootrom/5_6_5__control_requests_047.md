---
source_pdf: rp2350-datasheet-7.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.6.5. Control requests
pages: 411-413
type: technical_spec
generated_at: 2026-03-01T06:07:53.318745+00:00
---

# 5.6.5. Control requests

5.6.5. Control requests

The following requests are sent to the interface via the default control pipe.

5.6.5.1. INTERFACE_RESET (0x41)

The host sends this control request to reset the PICOBOOT interface. This command:

• Clears the HALT condition (if set) on each of the bulk endpoints
• Aborts any in-process PICOBOOT or Mass Storage transfer and any flash write (this method is the only way to kill a

stuck flash transfer).
• Clears the previous command result
• Removes EXCLUSIVE_ACCESS and remounts the Mass Storage drive if it was ejected due to exclusivity.

5.6. USB PICOBOOT interface
410

RP2350 Datasheet

| bmRequestType | bRequest | wValue | wIndex | wLength | Data |
| --- | --- | --- | --- | --- | --- |
| 01000001b | 01000001b | 0000h | Interface | 0000h | none |

Table 469. PICOBOOT

Reset PICOBOOT

interface control

This command responds with an empty packet on success.

5.6.5.2. GET_COMMAND_STATUS (0x42)

Retrieve the status of the last command (which may be a command still in progress). Successful completion of a

PICOBOOT Protocol Command is acknowledged over the bulk pipe, however if the operation is still in progress or has

failed (stalling the bulk pipe), then this method can be used to determine the operation’s status.

| bmRequestType | bRequest | wValue | wIndex | wLength | Data |
| --- | --- | --- | --- | --- | --- |
| 11000001b | 01000010b | 0000h | Interface | 0000h | none |

Table 470. PICOBOOT

Get last command

status control

The command responds with the following 16 byte response

| Offset | Name | Description |  |
| --- | --- | --- | --- |
| 0x00 | dToken | The user token specified with | the command |

Table 471. PICOBOOT

Get last command

status control

response

5.6. USB PICOBOOT interface
411

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

## Embedded Images

![img_p0411_00.png](images/img_p0411_00.png)

