---
source_pdf: rp2350-datasheet-5.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.13.3. Data size and endianness
pages: 1223-1223
type: technical_spec
generated_at: 2026-03-01T04:13:03.328193+00:00
---

# 12.13.3. Data size and endianness

12.13.3. Data size and endianness

Data is sent in message blocks of 512 bits, padded as described in Section 12.13.1. The SHA-256 accelerator updates

its 256-bit output state for each input block. The SHA-256 algorithm is defined in terms of big-endian message words,

but this accelerator provides a byte swap function via CSR.BSWAP to support little-endian data. BSWAP is set by default.

For more information, see the register descriptions.

WDATA supports 8-bit, 16-bit and 32-bit writes. The bus interface accumulates 8 and 16-bit writes in a 32-bit shift

register before passing them into the SHA-256 algorithm core. This means you must take care when mixing writes of

different sizes, because taking the shift register level from less than to greater than 32 bits in a single write will silently

drop data. You can avoid this issue by not mixing WDATA write sizes within a single SHA-256 message block (64 bytes).
