---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.7.8. Volume label simple example
pages: 415-415
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 5.7.8. Volume label simple example

![Page 415 figure](images/fig_p0415.png)

RP2350 Datasheet

5.7.6. UF2 INFO_UF2.TXT file

This is of the form:

UF2 Bootloader v1.0

Model: MODEL

Board-ID: BOARD_ID

• MODEL (default "Raspberry Pi RP2350", max-length 127 ASCII chars)
• BOARD_ID (default "RP2350", max-length 127 ASCII chars)

5.7.7. SCSI Inquiry

Returned via the SCSI Inquiry command:

• VENDOR (default "RPI", max-length 8 ASCII chars)
• PRODUCT (default "RP2350", max-length 16 ASCII chars)
• VERSION (default "1", max-length 4 ASCII chars)

5.7.8. Volume label simple example

Newer versions of picotool can load white-label data from a JSON file using the picotool otp white-label -s <start row>

<JSON filename> command. An example JSON file to set the volume label to "SPOON" would be:

{

    "volume": {

        "label": "SPOON"

    }

}

The <start row> is the OTP row where the white-label structure will be written - for example 0x400.

The full set of white-label fields which can be written using a JSON file are shown below. The manufacturer, product and

serial_number fields support Unicode characters, if you need special characters or emoji in your product name, but this

will take up twice as much room per character in the OTP for that field.

{

    "device": {

        "vid": "0x2e8b",

        "pid": "0x000e",

        "bcd": 2.15,

        "lang_id": "0x0c09",

        "manufacturer": "Test's Pis",

        "product": "Test RP2350?",

        "serial_number": "notnecessarilyanumber",

        "max_power": "0x20",

        "attributes": "0xe0"

    },

    "scsi": {

        "vendor": "TestPi",

        "product": "MyPi",

        "version": "v897"

    },

5.7. USB white-labelling
414
