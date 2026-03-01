---
source_pdf: rp2350-datasheet-8.pdf
repository: llm_database
chapter: Chapter 13. OTP
section: Chapter 13. OTP
pages: 1269-1269
type: technical_spec
generated_at: 2026-03-01T08:45:43.944040+00:00
---

# Chapter 13. OTP

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
