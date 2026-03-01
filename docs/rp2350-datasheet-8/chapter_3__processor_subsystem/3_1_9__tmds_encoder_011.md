---
source_pdf: rp2350-datasheet-8.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.1.9. TMDS encoder
pages: 45-45
type: technical_spec
generated_at: 2026-03-01T08:45:43.944040+00:00
---

# 3.1.9. TMDS encoder

3.1.9. TMDS encoder

Each core is equipped with an implementation of the TMDS encode algorithm described in chapter 3 of the DVI 1.0

specification. In general, the HSTX peripheral (Section 12.11) supports lower processor overhead for DVI-D output as

well as a wider range of pixel formats, but the SIO TMDS encoders are included for use with non-HSTX-capable GPIOs.

The TMDS_CTRL register allows configuration of a number of input pixel formats, from 16-bit RGB down to 1-bit

monochrome. Once the encoder has been set up, the processor writes 32 bits of colour data at a time to TMDS_WDATA,

and then reads TMDS data symbols from the output registers. Depending on the pixel format, there may be multiple

TMDS symbols read for each write to TMDS_WDATA. There are no stalls: encoding is limited entirely by the processor’s

load/store bandwidth, up to one 32-bit read or write per cycle per core.

To allow for framebuffer/scanbuffer resolution lower than the display resolution, the output registers have both peek

and pop aliases (e.g. TMDS_PEEK_SINGLE and TMDS_POP_SINGLE). Reading either register advances the encoder’s DC

balance counter, but only the pop alias shifts the colour data in TMDS_WDATA so that multiple correctly-DC-balanced

TMDS symbols can be generated from the same input pixel.

The TMDS encoder peripherals are not duplicated over security domains. They are assigned to the Secure SIO at reset,

and can be reassigned to the Non-secure SIO using the PERI_NONSEC register.
