---
source_pdf: rp2350-datasheet-9.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.7.8. Volume label simple example
pages: 415-416
type: technical_spec
generated_at: 2026-03-01T11:44:31.548839+00:00
---

# 5.7.8. Volume label simple example

5.7.8. Volume label simple example

Newer versions of picotool can load white-label data from a JSON file using the picotool otp white-label -s <start row>

<JSON filename> command. An example JSON file to set the volume label to "SPOON" would be:

```c
{
    "volume": {
        "label": "SPOON"
    }
}
```

The <start row> is the OTP row where the white-label structure will be written - for example 0x400.

The full set of white-label fields which can be written using a JSON file are shown below. The manufacturer, product and

serial_number fields support Unicode characters, if you need special characters or emoji in your product name, but this

will take up twice as much room per character in the OTP for that field.

```c
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
```

5.7. USB white-labelling
414

RP2350 Datasheet

```c
    "volume": {
        "label": "TestPi Boot",
        "redirect_url": "https://datasheets.raspberrypi.com/rp2350/rp2350-datasheet.pdf",
        "redirect_name": "The datasheet",
        "model": "My Test Pi",
        "board_id": "TPI-RP2350"
    }
}
```
