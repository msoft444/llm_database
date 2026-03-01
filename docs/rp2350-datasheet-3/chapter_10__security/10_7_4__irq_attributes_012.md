---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 10. Security
section: 10.7.4. IRQ attributes
pages: 869-869
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 10.7.4. IRQ attributes

RP2350 Datasheet

resources. It is also possible to assign the entire DMA block wholesale to a single security domain using the

ACCESSCTRL registers (Section 10.6) if this fine-grained configuration is not desired.

This section gives an overview of the DMA’s security features. The specific hardware details are documented in Section

12.6.6.

10.7.1. Channel security attributes

Each channel is assigned a security level using the per-channel registers starting at SECCFG_CH0. This defines:

• The minimum privilege required to configure and control the channel, or observe its status
• The bus privilege at which the channel performs its memory accesses

For the sake of comparing security levels, the DMA assigns the following total order to AHB5 security/privilege

attributes: Secure + Privileged > Secure + Unprivileged > Non-secure + Privileged > Non-secure + Unprivileged.

A channel’s security level can be changed freely up until any of the channel’s control registers is written. After this point,

its security level is locked, and cannot be changed until the DMA block resets. At reset, all channels become Secure +

Privileged (security level = 3, the maximum).

The effects of the channel SECCFG registers are listed exhaustively in the relevant DMA documentation, Section 12.6.6.1.

10.7.2. Memory protection unit

The RP2350 DMA features a memory protection unit that you can configure to set the security/privilege level required to

access up to eight different address ranges, plus a default level for addresses not matched by any of those eight

ranges. The addresses of all DMA reads and writes are checked against the MPU address map. If the originating

channel’s security level is lower than that defined in the address map, the access is filtered. A filtered access has no

effect on the downstream bus, and returns a bus error to the offending channel.

The DMA memory protection unit is configured by DMA control registers starting from MPU_CTRL. See Section 12.6.6.3

for more details.

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

10.7.4. IRQ attributes

Each of the four shared DMA interrupt lines (IRQs) has a configurable security level. The IRQ’s security level is

compared with channel security levels, and with the bus privilege of accesses to the DMA’s interrupt control registers, to

determine:

• Whether a bus access is permitted to read/write the INTE/INTF/INTS registers for this IRQ
• Whether a given channel will be visible in this IRQ’s INTS register (and therefore whether that channel will cause

10.7. DMA
868
