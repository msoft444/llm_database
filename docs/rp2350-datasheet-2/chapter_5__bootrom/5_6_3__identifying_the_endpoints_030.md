---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.6.3. Identifying the endpoints
pages: 405-405
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 5.6.3. Identifying the endpoints

![Page 405 figure](images/fig_p0405.png)

RP2350 Datasheet

| Field | Value |
| --- | --- |
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

5.6.2. Identifying the interface

The PICOBOOT interface is recognised by the vendor-specific Interface Class, the zero Interface Sub Class, and

Interface Protocol (shown in Table 457).

Don’t rely on the interface number, because that is dependent on whether the device is currently exposing the Mass

Storage Interface. The device might not be currently exposing the PICOBOOT interface at all, so you mustn’t assume it’s

present.

| Field | Value |
| --- | --- |
| bLength | 9 |
| bDescriptorType | 4 |
| bInterfaceNumber | varies |
| bAlternateSetting | 0 |
| bNumEndpoints | 2 |
| bInterfaceClass | 0xff (vendor specific) |
| bInterfaceSubClass | 0 |
| bInterfaceProtocol | 0 |
| iInterface | 0 |

Table 457. PICOBOOT

5.6.3. Identifying the endpoints

The PICOBOOT interface provides a single BULK_OUT and a single BULK_IN endpoint. These can be identified by their

direction and type. You mustn’t rely on endpoint numbers.

5.6. USB PICOBOOT interface
404
