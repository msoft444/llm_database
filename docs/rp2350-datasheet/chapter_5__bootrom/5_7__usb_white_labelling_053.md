---
source_pdf: rp2350-datasheet.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.7. USB white-labelling
pages: 413-414
type: technical_spec
generated_at: 2026-03-02T14:11:45.471392+00:00
---

# 5.7. USB white-labelling

To brand RP2350-based products, customers may replace identifying information exposed by USB interfaces. We call

this white-labelling, and you can accomplish it in RP2350 by specifying values in OTP.

1. Write the OTP location of the white-label data structure via USB_WHITE_LABEL_ADDR (see that register description

for the data structure contents).

2. Initialise the fields you wish to override in the white-label data structure and mark them valid.

3. Set USB_BOOT_FLAGS.WHITE_LABEL_ADDR_VALID to mark the white-labelling as valid.

The following fields can be modified:
