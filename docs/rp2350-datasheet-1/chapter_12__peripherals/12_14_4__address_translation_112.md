---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.14.4. Address translation
pages: 1235-1235
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 12.14.4. Address translation

Path
Max delay (ns) VDDIO=3.3V
Max delay (ns) VDDIO=1.8V
system clock to GPIO (QFN-60) output
3.5
4.9
system clock to GPIO (QFN-80) output
4.1
5.4
12.14.4. Address translation
QMI applies a configurable mapping from the virtual address requested by the processor or DMA to the physical
address transmitted to the external QSPI device. This is performed separately for each of the 16 MB chip select
windows. You cannot map contents between devices.
Each window is divided into four panes, each independently mapped onto the physical address space for that window.
The default configuration applied on QMI reset, as shown in Figure 140, is a 1:1 identity mapping between virtual and
physical addresses. In this state the address mapping has no effect, and the entire 16 MB address space of the external
QSPI device is mapped directly into the system address space.
Window 0
(Virtual)
Window 0
(Physical)
0 MB
4 MB
8 MB
12 MB
16 MB
0 MB
16 MB
Pane 0
ATRANS0
base=0
size=4M
ATRANS1
base=4M
size=4M
ATRANS2
base=8M
size=4M
ATRANS3
base=12M
size=4M
Pane 1
Pane 2
Pane 3
Figure 140. By default,
each window is set up
to map the full 16 MB
virtual address space
directly 1:1 with the
16 MB physical
address space.
Each pane corresponds to the one of the four ATRANSx registers for that window: ATRANS0 through ATRANS3 for window
0, and ATRANS4 through ATRANS7 for window 1.
The virtual base address of each pane is fixed and assigned in 4 MB increments. There are two configurable parameters
for the mapping of that pane into physical address space:
• BASE: defines the physical address corresponding to offset 0 in the virtual address pane. Configured in units of 4 kB
(one flash sector), ranging from 0 to (16 MB minus 4 kB).
• SIZE: defines the amount of address space mapped by this pane. Configured in units of 4 kB (one flash sector)
ranging from 0 to 4 MB.
The mapping grows from the start of the pane. A SIZE of 1 MB maps the first 1 MB of that pane’s virtual address range
to downstream memory, and the remainder is unmapped. A SIZE of 0 means that no address within this virtual address
pane is accessible. Accesses beyond the currently configured SIZE return a bus error, and do not pass through to the
downstream QSPI bus. As a result, they have no effect on the external memory device.
Window 0
(Virtual)
Window 0
(Physical)
0 MB
4 MB
8 MB
12 MB
16 MB
1 MB
5 MB
Pane 0
ATRANS0
base=1M
size=4M
ATRANS1
size=0
ATRANS2
size=0
ATRANS3
size=0
Pane 1
Pane 2
Pane 3
Figure 141. The BASE
of a pane defines
where its physical
mapping begins. The
SIZE defines how far it
extends. A SIZE of 0
means no addresses
are mapped through
that pane.
Figure 141 shows an example mapping, where the first 4 MB of virtual address space for chip select 0 (virtual address
offsets 0x000000 through 0x3fffff inclusive) map to a 4 MB physical address window starting at a 1 MB offset (physical
address offsets 0x100000 through 0x4fffff inclusive). This mapping could be used for flash that contains a 1 MB
RP2350 Datasheet
12.14. QSPI memory interface (QMI)
1234

