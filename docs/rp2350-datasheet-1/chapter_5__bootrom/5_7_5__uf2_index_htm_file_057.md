---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.7.5. UF2 INDEX.HTM file
pages: 414-414
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 5.7.5. UF2 INDEX.HTM file

1. Write the OTP location of the white-label data structure via USB_WHITE_LABEL_ADDR (see that register description
for the data structure contents).
2. Initialise the fields you wish to override in the white-label data structure and mark them valid.
3. Set USB_BOOT_FLAGS.WHITE_LABEL_ADDR_VALID to mark the white-labelling as valid.
The following fields can be modified:
5.7.1. USB device descriptor
The USB device descriptor includes the following 16-bit values:
• VID (default 0x2e8a)
• PID (default 0x000f)
• BCD_DEVICE (default 0x0100)
• LANG_ID (default 0x0409)
5.7.2. USB device strings
• MANUFACTURER (default "Raspberry Pi", max-length 30 UTF-16 or ASCII chars)
• PRODUCT (default "RP2350 Boot", max-length 30 UTF-16 or ASCII chars)
• SERIAL_NUMBER (default uppercase hex string of the device_id; first 4 rows of OTP, max-length 30 UTF-16 or ASCII
chars)
5.7.3. USB configuration descriptor
The USB Configuration Description isn’t strictly white-labelling, but is still helpful for users:
• ATTRIBUTES_MAX_POWER_VALUES (default 0xfa80, meaning bMaxPower of 0xfa and bmAttributes=0x80)
5.7.4. MSD drive
• VOLUME_LABEL (default "RP2350", max-length 11 ASCII chars)
5.7.5. UF2 INDEX.HTM file
This is of the form:
<html>
    <head>
        <meta http-equiv="refresh" content="0;URL='*REDIRECT_URL*'"/>
    </head>
    <body>Redirecting to <a href='`*REDIRECT_URL*'>`*REDIRECT_NAME*</a></body>
</html>
• REDIRECT_URL (default "https://raspberrypi.com/device/RP2?version=5A09D5312E22", note the 12 hex digits are the
first 6 of the SYSINFO_GITREF_RP2350 and the first 6 of the bootrom gitref, max-length 127 ASCII chars)
• REDIRECT_NAME (default "raspberrypi.com", max-length 127 ASCII chars)
RP2350 Datasheet
5.7. USB white-labelling
413

