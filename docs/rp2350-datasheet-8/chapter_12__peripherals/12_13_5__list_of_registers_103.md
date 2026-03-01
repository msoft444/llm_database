---
source_pdf: rp2350-datasheet-8.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.13.5. List of registers
pages: 1224-1227
type: technical_spec
generated_at: 2026-03-01T08:45:43.944040+00:00
---

# 12.13.5. List of registers

12.13.5. List of registers

The SHA-256 registers start at a base address of 0x400f8000 (defined as SHA256_BASE in SDK).

| Offset | Name | Info |
| --- | --- | --- |
| 0x00 | CSR | Control and status register |
| 0x04 | WDATA | Write data register |
| 0x08 | SUM0 | 256-bit checksum result. Contents are undefined when CSR_SUM_VLD is 0. |
| 0x0c | SUM1 | 256-bit checksum result. Contents are undefined when CSR_SUM_VLD is 0. |
| 0x10 | SUM2 | 256-bit checksum result. Contents are undefined when CSR_SUM_VLD is 0. |
| 0x14 | SUM3 | 256-bit checksum result. Contents are undefined when CSR_SUM_VLD is 0. |
| 0x18 | SUM4 | 256-bit checksum result. Contents are undefined when CSR_SUM_VLD is 0. |
| 0x1c | SUM5 | 256-bit checksum result. Contents are undefined when CSR_SUM_VLD is 0. |
| 0x20 | SUM6 | 256-bit checksum result. Contents are undefined when CSR_SUM_VLD is 0. |
| 0x24 | SUM7 | 256-bit checksum result. Contents are undefined when CSR_SUM_VLD is 0. |

Table 1280. List of

SHA256: CSR Register

Offset: 0x00

Description

Control and status register

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:13 | Reserved. | - | - |
| 12 | BSWAP: Enable byte swapping of 32-bit values at the point they are committed to the SHA message scheduler. This block’s bus interface assembles byte/halfword data into message words in little-endian order, so that DMAing the same buffer with different transfer sizes always gives the same result on a little-endian system like RP2350. However, when marshalling bytes into blocks, SHA expects that the first byte is the most significant in each message word. To resolve this, once the bus interface has accumulated 32 bits of data (either a word write, two halfword writes in little-endian order, or four byte writes in little-endian order) the final value can be byte-swapped before passing to the actual SHA core. This feature is enabled by default because using the SHA core to checksum byte buffers is expected to be more common than having preformatted SHA message words lying around. | RW | 0x1 |
| 11:10 | Reserved. | - | - |
| 9:8 | DMA_SIZE: Configure DREQ logic for the correct DMA data size. Must be configured before the DMA channel is triggered. The SHA-256 core’s DREQ logic requests one entire block of data at once, since there is no FIFO, and data goes straight into the core’s message schedule and digest hardware. Therefore, when transferring data with DMA, CSR_DMA_SIZE must be configured in advance so that the correct number of transfers can be requested per block. | RW | 0x2 |
|  | Enumerated values: |  |  |
|  | 0x0 → 8BIT |  |  |
|  | 0x1 → 16BIT |  |  |
|  | 0x2 → 32BIT |  |  |
| 7:5 | Reserved. | - | - |
| 4 | ERR_WDATA_NOT_RDY: Set when a write occurs whilst the SHA-256 core is not ready for data (WDATA_RDY is low). Write one to clear. | WC | 0x0 |
| 3 | Reserved. | - | - |
| 2 | SUM_VLD: If 1, the SHA-256 checksum presented in registers SUM0 through SUM7 is currently valid. Goes low when WDATA is first written, then returns high once 16 words have been written and the digest of the current 512-bit block has subsequently completed. | RO | 0x1 |
| 1 | WDATA_RDY: If 1, the SHA-256 core is ready to accept more data through the WDATA register. After writing 16 words, this flag will go low for 57 cycles whilst the core completes its digest. | RO | 0x1 |
| 0 | START: Write 1 to prepare the SHA-256 core for a new checksum. The SUMx registers are initialised to the proper values (fractional bits of square roots of first 8 primes) and internal counters are cleared. This immediately forces WDATA_RDY and SUM_VLD high. START must be written before initiating a DMA transfer to the SHA-256 core, because the core will always request 16 transfers at a time (1 512-bit block). Additionally, the DMA channel should be configured for a multiple of 16 32-bit transfers. | SC | 0x0 |

Table 1281. CSR

12.13. SHA-256 accelerator
1223

RP2350 Datasheet

12.13. SHA-256 accelerator
1224

RP2350 Datasheet

SHA256: WDATA Register

Offset: 0x04

Description

Write data register

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:0 | After pulsing START and writing 16 words of data to this register, WDATA_RDY will go low and the SHA-256 core will complete the digest of the current 512- bit block. Software is responsible for ensuring the data is correctly padded and terminated to a whole number of 512-bit blocks. After this, WDATA_RDY will return high, and more data can be written (if any). This register supports word, halfword and byte writes, so that DMA from non- word-aligned buffers can be supported. The total amount of data per block remains the same (16 words, 32 halfwords or 64 bytes) and byte/halfword transfers must not be mixed within a block. | WF | 0x00000000 |

Table 1282. WDATA

SHA256: SUM0 Register

Offset: 0x08

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:0 | 256-bit checksum result. Contents are undefined when CSR_SUM_VLD is 0. | RO | 0x00000000 |

Table 1283. SUM0

SHA256: SUM1 Register

Offset: 0x0c

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:0 | 256-bit checksum result. Contents are undefined when CSR_SUM_VLD is 0. | RO | 0x00000000 |
| 31:0 | 256-bit checksum result. Contents are undefined when CSR_SUM_VLD is 0. | RO | 0x00000000 |

Table 1284. SUM1

SHA256: SUM2 Register

Offset: 0x10

12.13. SHA-256 accelerator
1225

RP2350 Datasheet

Table 1285. SUM2

SHA256: SUM3 Register

Offset: 0x14

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:0 | 256-bit checksum result. Contents are undefined when CSR_SUM_VLD is 0. | RO | 0x00000000 |

Table 1286. SUM3

SHA256: SUM4 Register

Offset: 0x18

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:0 | 256-bit checksum result. Contents are undefined when CSR_SUM_VLD is 0. | RO | 0x00000000 |

Table 1287. SUM4

SHA256: SUM5 Register

Offset: 0x1c

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:0 | 256-bit checksum result. Contents are undefined when CSR_SUM_VLD is 0. | RO | 0x00000000 |

Table 1288. SUM5

SHA256: SUM6 Register

Offset: 0x20

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:0 | 256-bit checksum result. Contents are undefined when CSR_SUM_VLD is 0. | RO | 0x00000000 |

Table 1289. SUM6

SHA256: SUM7 Register

Offset: 0x24

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:0 | 256-bit checksum result. Contents are undefined when CSR_SUM_VLD is 0. | RO | 0x00000000 |

Table 1290. SUM7

12.14. QSPI memory interface (QMI)
