---
source_pdf: rp2350-datasheet.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.7.6. UF2 INFO_UF2.TXT file
pages: 415-415
type: technical_spec
generated_at: 2026-03-02T14:11:45.471392+00:00
---

# 5.7.6. UF2 INFO_UF2.TXT file

This is of the form:

```c
UF2 Bootloader v1.0
Model: MODEL
Board-ID: BOARD_ID
```

• MODEL (default "Raspberry Pi RP2350", max-length 127 ASCII chars)
• BOARD_ID (default "RP2350", max-length 127 ASCII chars)
