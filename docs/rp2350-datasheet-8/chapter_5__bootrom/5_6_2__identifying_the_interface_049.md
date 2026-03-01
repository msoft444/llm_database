---
source_pdf: rp2350-datasheet-8.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.6.2. Identifying the interface
pages: 405-405
type: technical_spec
generated_at: 2026-03-01T08:45:43.944040+00:00
---

# 5.6.2. Identifying the interface

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
