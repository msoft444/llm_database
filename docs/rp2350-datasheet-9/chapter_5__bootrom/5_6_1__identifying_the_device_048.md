---
source_pdf: rp2350-datasheet-9.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.6.1. Identifying the device
pages: 404-405
type: technical_spec
generated_at: 2026-03-01T11:44:31.548839+00:00
---

# 5.6.1. Identifying the device

5.6.1. Identifying the device

A RP2350 device can recognised by the Vendor ID and Product ID in its device descriptor (shown in Table 456), unless

different values have been set in OTP (see Section 5.7)

| Field | Value |
| --- | --- |
| bLength | 18 |
| bDescriptorType | 1 |
| bcdUSB | 2.10 |
| bDeviceClass | 0 |
| bDeviceSubClass | 0 |
| bDeviceProtocol | 0 |
| bMaxPacketSize0 | 64 |
| idVendor | 0x2e8a - this value may be overridden in OTP |
| idProduct | 0x000f - this value may be overridden in OTP |
| bcdDevice | 1.00 - this value may be overridden in OTP |
| iManufacturer | 1 |
| iProduct | 2 |
| iSerial | 3 |
| bNumConfigurations | 1 |

*Table 456. RP2350 Boot Device Descriptor*

5.6. USB PICOBOOT interface
403

RP2350 Datasheet
