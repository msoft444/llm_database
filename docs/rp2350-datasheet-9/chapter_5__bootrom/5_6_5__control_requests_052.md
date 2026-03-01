---
source_pdf: rp2350-datasheet-9.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.6.5. Control requests
pages: 411-413
type: technical_spec
generated_at: 2026-03-01T11:44:31.548839+00:00
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

*Table 469. PICOBOOT Reset PICOBOOT interface control*

This command responds with an empty packet on success.

5.6.5.2. GET_COMMAND_STATUS (0x42)

Retrieve the status of the last command (which may be a command still in progress). Successful completion of a

PICOBOOT Protocol Command is acknowledged over the bulk pipe, however if the operation is still in progress or has

failed (stalling the bulk pipe), then this method can be used to determine the operation’s status.

| bmRequestType | bRequest | wValue | wIndex | wLength | Data |
| --- | --- | --- | --- | --- | --- |
| 11000001b | 01000010b | 0000h | Interface | 0000h | none |

*Table 470. PICOBOOT Get last command status control*

The command responds with the following 16 byte response

| Offset | Name | Description |  |
| --- | --- | --- | --- |
| 0x00 | dToken | The user token specified with | the command |

*Table 471. PICOBOOT Get last command status control response*

5.6. USB PICOBOOT interface
411

RP2350 Datasheet

![Page 413 figure](images/fig_p0413.png)

## Embedded Images

![img_p0411_00.png](images/img_p0411_00.png)

