---
source_pdf: rp2350-datasheet-5.pdf
repository: llm_database
chapter: Chapter 10. Security
section: 10.7.3. DREQ attributes
pages: 869-869
type: technical_spec
generated_at: 2026-03-01T04:13:03.328193+00:00
---

# 10.7.3. DREQ attributes

10.7.3. DREQ attributes

Channels are not permitted to interface with the DREQs of peripherals above their security level, as determined by the

peripheral access controls in ACCESSCTRL. This is done to avoid any information being inferred from the timing of

secure peripheral transfers, and because the clear handshake on the RP2040 DREQ can be used maliciously to cause a

Secure DMA channel to overflow its destination FIFO and corrupt/lose data (for details about the DREQ handshake, see

Section 12.6.4.2).

The DREQ security levels are driven by the ACCESSCTRL block access registers. ACCESSCTRL takes the index of the

least-significant set bit in the 4-bit permission mask, having first ANDed the SP into SU, and NSP into SU. This creates a 2-

bit integer which is compared with the DMA channel’s security level to determine whether it can interface with this

DREQ.
