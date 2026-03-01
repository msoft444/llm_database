---
source_pdf: rp2350-datasheet-7.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.2.5. RAM image boot
pages: 373-373
type: technical_spec
generated_at: 2026-03-01T06:07:53.318745+00:00
---

# 5.2.5. RAM image boot

5.2.5. RAM image boot

The bootrom is directed (through values in the watchdog registers) to boot into an image in SRAM or XIP SRAM. The

two parameters indicate the start and size of the region to search for a block loop containing a valid (and correctly

signed if necessary) IMAGE_DEF. These are passed as parameter 0/1, in watchdog scratch 2/3.

If the image to be booted is contained in XIP SRAM, the XIP SRAM must be pinned in place by the bootrom prior to

launch. For this reason, if you are using XIP SRAM for your binaries, you must add a special entry to the LOAD_MAP item

(see Section 5.9.3.2).

5.2. Processor-controlled boot sequence
372
