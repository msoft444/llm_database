---
source_pdf: rp2350-datasheet-7.pdf
repository: llm_database
chapter: Appendix B: Units used in this document
section: Digit Separators
pages: 1354-1355
type: technical_spec
generated_at: 2026-03-01T06:07:53.318745+00:00
---

# Digit Separators

Digit Separators

Numbers written out with many digits may have either commas or spaces inserted for easier reading:

• 1,000,000: one million
• 1 000 000: one million

A comma in a number never represents a decimal (radix) point.

Scale Prefixes
1353

RP2350 Datasheet

Appendix C: Hardware revision
history

This appendix summarises the differences between RP2350 hardware revisions, referred to as steppings. To determine

the stepping of an unknown device, check the package markings, as described in Section 14.4. Software running on the

device can also read the CHIP_ID.REVISION register field, or call the rp2350_chip_version() SDK function.

In this appendix:

• The term fix refers to an issue that’s fully resolved.
• The term mitigate refers to an issue that’s either partially resolved or believed to be fully resolved but with an

unpredictable underlying cause, such as a fault injection vulnerability.
• The term update refers to any other difference between steppings.

This appendix offers a high-level overview; for detailed information, refer to individual errata entries in appendix E.
