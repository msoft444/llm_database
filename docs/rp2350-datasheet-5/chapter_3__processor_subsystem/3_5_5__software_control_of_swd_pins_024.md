---
source_pdf: rp2350-datasheet-5.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.5.5. Software control of SWD pins
pages: 88-88
type: technical_spec
generated_at: 2026-03-01T04:13:03.328193+00:00
---

# 3.5.5. Software control of SWD pins

3.5.5. Software control of SWD pins

The DBGFORCE register in SYSCFG can be used to detach the SW-DP from the external debug pads, and instead bitbang

the internal SWD signals directly from software. This is intended for a debug probe running on one core being used to

debug the other core. For other use cases it is generally cleaner to use the self-hosted debug access to interface with

the APs directly from the system bus.
