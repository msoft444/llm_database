---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.13.4. DMA DREQ interface
pages: 1223-1223
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 12.13.4. DMA DREQ interface

RP2350 Datasheet

3. Write additional trailer and padding data to WDATA, as described in Section 12.13.1 below.

4. Poll CSR.SUM_VLD to wait for the last block to be digested.

5. Read the 256-bit result from the 8 read-only result registers starting at SUM0.

12.13.1. Message padding

Pad message content according to the standard SHA-256 method as described in the FIPS 180-4 Secure Hash

Standard: append the message with single bit 1, then a number of 0 bits, then a 64-bit count of the number of message

bits. So for a message M with length L bits the padded message should be:

1. message M

2. 1

3. k zero bits, where k is the smallest non-negative solution to the equation: L + 1 + k = 448 mod 512

4. a 64-bit block indicating L (the length of the message) in binary

For example, the 8 bit ASCII message abc has a length of 24 bits. This is padded with 1, then 448-(24+1) = 423 0 bits, and

then the message length as a 64-bit value as follows:

01100001 01100010 01100011 1 00000000 000...0 00000000 000...0 00011000

|---------message--------| 1 |--423 0 bits--| |------64 bit len-------|

12.13.2. Throughput

SHA-256 processes data one 512-bit block at a time. This requires 16 32-bit writes, 32 16-bit writes, or 64 8-bit writes to

the WDATA register. An APB register write costs 4 cycles, so it takes at least 64 system clock cycles to write a data

block.

Once a full block is transferred, the SHA core takes a further 57 cycles to complete the block digest. CSR.WDATA_RDY

goes low, and you must not write to WDATA during this time.

The maximum throughput is therefore one block per 121 system clock cycles, or 0.53 bytes per cycle. At a 150 MHz

system clock this is 79.3 MB/s. This throughput is achieved when you use 32-bit transfers from the DMA. Using

narrower transfers result in lower throughput, as does polling the CSR.WDATA_RDY flag when transferring data from the

processor.

12.13.3. Data size and endianness

Data is sent in message blocks of 512 bits, padded as described in Section 12.13.1. The SHA-256 accelerator updates

its 256-bit output state for each input block. The SHA-256 algorithm is defined in terms of big-endian message words,

but this accelerator provides a byte swap function via CSR.BSWAP to support little-endian data. BSWAP is set by default.

For more information, see the register descriptions.

WDATA supports 8-bit, 16-bit and 32-bit writes. The bus interface accumulates 8 and 16-bit writes in a 32-bit shift

register before passing them into the SHA-256 algorithm core. This means you must take care when mixing writes of

different sizes, because taking the shift register level from less than to greater than 32 bits in a single write will silently

drop data. You can avoid this issue by not mixing WDATA write sizes within a single SHA-256 message block (64 bytes).

12.13.4. DMA DREQ interface

The block can request the DMA controller to send entire blocks of data at once. Configure transfer size using

12.13. SHA-256 accelerator
1222
