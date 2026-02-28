---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.1.6. Doorbells
pages: 43-43
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 3.1.6. Doorbells

NOTE
Due to RP2350-E2, writes to new SIO registers above an offset of +0x180 alias the spinlocks, causing spurious lock
releases. The SDK by default uses atomic memory accesses to implement the hardware_sync_spin_lock API, as a
workaround on RP2350 A2.
3.1.5. Inter-processor FIFOs (Mailboxes)
The SIO contains two FIFOs for passing data, messages or ordered events between the two cores. Each FIFO is 32 bits
wide and four entries deep. One of the FIFOs can only be written by core 0 and read by core 1. The other can only be
written by core 1 and read by core 0.
Each core writes to its outgoing FIFO by writing to FIFO_WR and reads from its incoming FIFO by reading from FIFO_RD.
A status register, FIFO_ST, provides the following status signals:
• Incoming FIFO contains data (VLD).
• Outgoing FIFO has room for more data (RDY).
• The incoming FIFO was read from while empty at some point in the past (ROE).
• The outgoing FIFO was written to while full at some point in the past (WOF).
Writing to the outgoing FIFO while full, or reading from the incoming FIFO while empty, does not affect the FIFO state.
The current contents and level of the FIFO is preserved. However, this does represent some loss of data or reception of
invalid data by the software accessing the FIFO, so a sticky error flag is raised (ROE or WOF).
The SIO has a FIFO IRQ output for each core to notify the core that it has received FIFO data. This is a core-local
interrupt, mapped to the same IRQ number on each core (SIO_IRQ_FIFO, interrupt number 25). Non-secure FIFO interrupts
use a separate interrupt line, (SIO_IRQ_FIFO_NS, interrupt number 27). It is not possible to interrupt on the opposite core’s
FIFO.
Each IRQ output is the logical OR of the VLD, ROE and WOF bits in that core’s FIFO_ST register: that is, the IRQ is asserted if
any of these three bits is high, and clears again when they are all low. To clear the ROE and WOF flags, write any value to
FIFO_ST. To clear the VLD flag, read data from the FIFO until it is empty.
If the corresponding interrupt line is enabled in the processor’s interrupt controller, the processor takes an interrupt
each time data appears in its FIFO, or if it has performed some invalid FIFO operation (read on empty, write on full).
NOTE
ROE and WOF only become set if software misbehaves in some way. Generally, the interrupt handler triggers when data
appears in the FIFO, raising the VLD flag. Then, the interrupt handler clears the IRQ by reading data from the FIFO until
VLD goes low once more.
The inter-processor FIFOs and the Event signals are used by the bootrom (Chapter 5) wait_for_vector routine, where core
1 remains in a sleep state until it is woken, and provided with its initial stack pointer, entry point and vector table through
the FIFO.
NOTE
RP2350 has separate FIFOs and interrupts for Secure and Non-secure SIO banks. See Section 3.1.1
3.1.6. Doorbells
The doorbell registers raise an interrupt on the opposite core. There are 8 doorbell flags in each direction, combined into
a single doorbell interrupt per core. This is a core-local interrupt: the same interrupt number on each core (SIO_IRQ_BELL,
interrupt number 26) notifies that core of incoming doorbell interrupts.
RP2350 Datasheet
3.1. SIO
42

