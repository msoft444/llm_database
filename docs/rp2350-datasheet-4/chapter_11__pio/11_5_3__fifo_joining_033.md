---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 11. PIO
section: 11.5.3. FIFO joining
pages: 906-906
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 11.5.3. FIFO joining

11.5.3. FIFO joining

By default, each state machine possesses a 4-entry FIFO in each direction: one for data transfer from system to state

machine (TX), the other for the reverse direction (RX). However, many applications do not require bidirectional data

transfer between the system and an individual state machine, but may benefit from deeper FIFOs: in particular, high-

bandwidth interfaces such as DPI. For these cases, SHIFTCTRL_FJOIN can merge the two 4-entry FIFOs into a single 8-entry

FIFO.

11.5. Functional details
905
