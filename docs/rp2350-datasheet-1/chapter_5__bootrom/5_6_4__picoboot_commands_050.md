---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.6.4. PICOBOOT Commands
pages: 406-410
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 5.6.4. PICOBOOT Commands

5.6.4. PICOBOOT Commands
The two bulk endpoints are used for sending commands and retrieved successful command results. All commands are
exactly 32 bytes (see Table 458) and sent to the BULK_OUT endpoint.
Table 458. PICOBOOT
Command Definition
Offset
Name
Description
0x00
dMagic
The value 0x431fd10b
0x04
dToken
A user provided token to identify this request by
0x08
bCmdId
The ID of the command. Note that the top bit indicates data transfer direction
(0x80 = IN)
0x09
bCmdSize
Number of bytes of valid data in the args field
0x0a
reserved
0x0000
0x0c
dTransferLength
The number of bytes the host expects to send or receive over the bulk channel
0x10
args
16 bytes of command-specific data padded with zeros
If a command sent is invalid or not recognised, the bulk endpoints will be stalled. Further information will be available
via the GET_COMMAND_STATUS request (see Section 5.6.5.2).
Following the initial 32 byte packet, if dTransferLength is non-zero, then that many bytes are transferred over the bulk pipe
and the command is completed with an empty packet in the opposite direction. If dTransferLength is zero then command
success is indicated by an empty IN packet.
The following commands are supported (note common fields dMagic, dToken, and reserved are omitted for clarity)
5.6.4.1. EXCLUSIVE_ACCESS (0x01)
Claim or release exclusive access for writing to the RP2350 over USB (versus the Mass Storage Interface)
Table 459. PICOBOOT
EXCLUSIVE_ACCESS
command structure
Offset
Name
Value / Description
0x08
bCmdId
0x01 (EXCLUSIVE_ACCESS)
0x09
bCmdSize
0x01
0x0c
dTransferLength
0x00000000
0x10
bExclusive
NOT_EXCLUSIVE (0)
No restriction on USB Mass Storage operation
EXCLUSIVE (1)
Disable USB Mass Storage writes (the host should
see them as write protect failures, but in any case
any active UF2 download will be aborted)
EXCLUSIVE_AND_EJECT (2)
Lock the USB Mass Storage Interface out by
marking the drive media as not present (ejecting
the drive)
5.6.4.2. REBOOT (0x02)
Not supported on RP2350.
Use Section 5.6.4.10 instead.
RP2350 Datasheet
5.6. USB PICOBOOT interface
405


5.6.4.3. FLASH_ERASE (0x03)
Erases a contiguous range of flash sectors.
Table 460. PICOBOOT
FLASH_ERASE
command structure
Offset
Name
Value / Description
0x08
bCmdId
0x03 (FLASH_ERASE)
0x09
bCmdSize
0x08
0x0c
dTransferLength
0x00000000
0x10
dAddr
The address in flash to erase, starting at this location. This must be sector
(4 kB) aligned
0x14
dSize
The number of bytes to erase. This must an exact multiple number of sectors
(4 kB)
5.6.4.4. READ (0x84)
Read a contiguous memory (flash or RAM or ROM) range from the RP2350
Table 461. PICOBOOT
Read memory
command (flash, RAM,
ROM) structure
Offset
Name
Value / Description
0x08
bCmdId
0x84 (READ)
0x09
bCmdSize
0x08
0x0c
dTransferLength
Must be the same as dSize
0x10
dAddr
The address to read from. May be in flash or RAM or ROM
0x14
dSize
The number of bytes to read
5.6.4.5. WRITE (0x05)
Writes a contiguous memory range of memory (flash or RAM) on the RP2350.
Table 462. PICOBOOT
Write memory
command (flash,
RAM) structure
Offset
Name
Value / Description
0x08
bCmdId
0x05 (WRITE)
0x09
bCmdSize
0x08
0x0c
dTransferLength
Must be the same as dSize
0x10
dAddr
The address to write from. May be in flash or RAM, however must be page
(256 byte) aligned if in flash. Flash must be erased first or the results are
undefined.
0x14
dSize
The number of bytes to write. If writing to flash and the size isn’t an exact
multiple of pages (256 bytes) then the last page is zero-filled to the end.
5.6.4.6. EXIT_XIP (0x06)
A no-op provided for compatibility with RP2040. An XIP exit sequence (flash_exit_xip()) is issued once before entering
the USB bootloader, which returns the external QSPI device from whatever XIP state it was in to a serial command state,
and the external QSPI device then remains in this state until reboot.
RP2350 Datasheet
5.6. USB PICOBOOT interface
406


Table 463. PICOBOOT
EXIT_XIP command
structure
Offset
Name
Value / Description
0x08
bCmdId
0x06 (EXIT_XIP)
0x09
bCmdSize
0x00
0x0c
dTransferLength
0x00000000
5.6.4.7. ENTER_XIP (0x07)
A no-op provided for compatibility with RP2040. Note that, unlike RP2040, the low-level bootrom flash operations do not
leave the QSPI interface in a state where XIP is inaccessible, therefore there is no need to reinitialise the interface each
time. XIP setup is performed once before entering the USB bootloader, using an 03h command with a fixed clock divisor
of 6.
Table 464. PICOBOOT
Enter Execute in place
(XIP) command
Offset
Name
Value / Description
0x08
bCmdId
0x07 (ENTER_XIP)
0x09
bCmdSize
0x00
0x0c
dTransferLength
0x00000000
5.6.4.8. EXEC (0x08)
Not supported on RP2350.
5.6.4.9. VECTORIZE_FLASH (0x09)
Not supported on RP2350.
5.6.4.10. REBOOT2 (0x0a)
Reboots the RP2350 out of BOOTSEL mode. Note that BOOTSEL mode may be re-entered if no valid bootable image is
found.
The parameters flags, delay_ms, p0, p1 are the same as for api_reboot()
Table 465. PICOBOOT
REBOOT2 command
structure
Offset
Name
Value / Description
0x08
bCmdId
0x0a (REBOOT2)
0x09
bCmdSize
0x10
0x0c
dTransferLength
0x00000000
0x10
dAddr
flags
0x14
dSize
delay_ms
0x18
dSize
p0
0x1c
dSize
p1
5.6.4.11. GET_INFO (0x8b)
Generic conduit for retrieving information from the device.
RP2350 Datasheet
5.6. USB PICOBOOT interface
407


The transfer length indicates the maximum number of bytes to be retrieved. The fist word returned indicates the number
of significant words of data that follow. A full "transfer length" is always returned, padding with zeroes as necessary.
"Word 0", below, refers to the first word of the actual response (the word after the count word).
Table 466. PICOBOOT
GET_INFO command
structure
Offset
Name
Value / Description
0x08
bCmdId
0x0b (GET_INFO)
0x09
bCmdSize
0x10
0x0c
dTransferLength
the size of data to be received. Note this must be a multiple of 4, and less than
256
RP2350 Datasheet
5.6. USB PICOBOOT interface
408


Offset
Name
Value / Description
0x10
bType
the type of information being retrieved:
• 0x1 - INFO_SYS : Retrieves information from get_sys_info(); the flag
parameter for that function comes from dParam0.
• 0x2 - PARTITION : Retrieves information from get_partition_table_info(); the
flags_and_partition parameter for that function comes from dParam0.
• 0x03 - UF2_TARGET_PARTITION : Retrieves the partition that a given UF2
family_id would be downloaded into (if it were dragged on the USB drive
in BOOTSEL mode). The family id is passed in dParam0.
◦Word 0 : Target partition number:
▪0-15 : the partition number the family would be downloaded to
▪0xff : if the family would be downloaded at an absolute
location
▪-1 : if there is nowhere to download the family
◦Word 1 : Target partition Section 5.9.4.2 if the partition number is
not -1
◦Word 2 : Target partition Section 5.9.4.2 if the partition number is
not -1
• 0x04 - UF2_STATUS : Retrieves information about the current/recent UF2
download
◦Word 0 - 0xnnrr00af
▪'n' - no reboot flag; if 0x01, there is no reboot when the UF2
download completes
▪'r' - if 0x01, the UF2 being download is a RAM UF2
▪'a' - UF2 download abort reason flags
▪0x1 EXCLUSIVELY_LOCKED
▪0x2 BAD_ADDRESS
▪0x4 WRITE_ERROR
▪0x8 REBOOT_FAILURE // if the UF2 targeted a disabled
architecture
▪'f' - UF2 download status flags
▪0x1 IGNORED_FAMILY
◦Word 1 - the current family id
◦Word 2 - the number of 256 byte blocks successfully downloaded
◦Word 3 - the total number of 256 byte blocks in the UF2 to download
5.6.4.12. OTP_READ (0x8c)
Reads data out of OTP. (see also otp_access() which provides the data). Data returned is subject to the "BL" OTP
permissions, which define bootloader OTP access permissions.
RP2350 Datasheet
5.6. USB PICOBOOT interface
409

