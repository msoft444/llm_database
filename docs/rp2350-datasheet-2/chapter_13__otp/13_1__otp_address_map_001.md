---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 13. OTP
section: 13.1. OTP address map
pages: 1269-1269
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 13.1. OTP address map

RP2350 Datasheet

Chapter 13. OTP

RP2350 provides 8 kB of one-time programmable storage (OTP), which holds:

• Preprogrammed per-device information, such as unique device identifier and oscillator trim values
• Security configuration such as debug disable and secure boot enable
• Public key fingerprints for secure boot
• Symmetric keys for decryption of flash contents into SRAM
• Configuration for the USB bootloader, such as customising VID/PID and descriptors
• Bootable software images, for low-cost flashless applications or custom bootloaders
• Any other user-defined data, such as per-device personalisation values

For the full listing of predefined OTP contents, see Section 13.10.

OTP is physically an array of 4096 rows of 24 bits each. You can directly access these 24-bit values, but there is also

hardware support for storing 16 bits of data in each row, with 6 bits of Hamming ECC protection and 2 bits of bit polarity

reversal protection, yielding an ECC data capacity of 8192 bytes.

On a blank device, the OTP contents is all zeroes, except for some basic device information pre-programmed during

manufacturing test. Each bit can be irreversibly programmed from zero to one. To program the OTP contents:

• Directly access the registers using the SBPI bridge
• Call the bootrom otp_access API (Section 5.4.8.21)
• Use the PICOBOOT interface of the USB bootloader (Section 5.6)

RP2350 enforces page-based permissions on OTP to partition Secure from Non-secure data and to ensure that

contents that should not change do not change. The OTP address space is logically partitioned into 64 pages, each 64

rows in size, for a total of 128 bytes of ECC data per page. Pages initially have full read-write permissions, but can be

restricted to read-only or inaccessible for each of Secure, Non-secure and bootloader access.

The page permissions themselves are stored in OTP. Locking pages in this way is an irreversible operation, referred to

as hard locking. The hardware also supports soft locking, where a page’s permissions are further restricted by writing to

the relevant register in SW_LOCK0 through SW_LOCK63; this restriction remains in effect until the next OTP reset.

Resetting the OTP block also resets the processors, so soft locking can be used to restrict the availability of sensitive

content like decryption keys to early boot stages.

OTP access keys (Section 13.5.2) provide an additional layer of protection. A fixed challenge is written to a write-only

OTP area. Pages registered to that key require the key to be entered to a write-only register in order to open read or write

access. This supports configuration data that can be accessed or edited by the board manufacturer, but not by general

firmware running on the device.

13.1. OTP address map

The OTP hardware resides in a 128 kB region starting at 0x40120000 (OTP_BASE in the SDK). Bit 16 of the address is used

to select either the OTP control registers, in the lower 64 kB, or one of the OTP read data aliases, in the upper 64 kB of

this space.

The OTP control registers (Section 13.9) are aliased at 4 kB intervals to implement the usual set, clear, and XOR atomic

write aliases described in Section 2.1.3.

The read data region starting at 0x40130000 divides further into four aliases:

• 0x40130000, OTP_DATA_BASE: ECC read alias. A 32-bit read returns the ECC-corrected data for two neighbouring rows, or

all-ones on permission failure. Only the first 8 kB is populated.

13.1. OTP address map
1268
