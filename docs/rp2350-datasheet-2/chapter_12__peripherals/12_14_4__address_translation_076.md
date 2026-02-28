---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.14.4. Address translation
pages: 1235-1235
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 12.14.4. Address translation

![Page 1235 figure](images/fig_p1235.png)

RP2350 Datasheet

| Path | Max delay (ns) VDDIO=3.3V | Max delay (ns) VDDIO=1.8V |
| --- | --- | --- |
| system clock to GPIO (QFN-60) output | 3.5 | 4.9 |
| system clock to GPIO (QFN-80) output | 4.1 | 5.4 |

12.14.4. Address translation

QMI applies a configurable mapping from the virtual address requested by the processor or DMA to the physical

address transmitted to the external QSPI device. This is performed separately for each of the 16 MB chip select

windows. You cannot map contents between devices.

Each window is divided into four panes, each independently mapped onto the physical address space for that window.

The default configuration applied on QMI reset, as shown in Figure 140, is a 1:1 identity mapping between virtual and

physical addresses. In this state the address mapping has no effect, and the entire 16 MB address space of the external

QSPI device is mapped directly into the system address space.

Figure 140. By default,

0 MB
4 MB
8 MB
12 MB
16 MB

each window is set up

Pane 0 | Pane 1 | Pane 2 | Pane 3

to map the full 16 MB

Window 0

(Virtual)

virtual address space

directly 1:1 with the

ATRANS0

ATRANS1

ATRANS2

ATRANS3
base=12M

16 MB physical

base=0
size=4M

base=4M

base=8M

address space.

size=4M

size=4M

size=4M

Window 0
(Physical)

0 MB
16 MB

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

Figure 141. The BASE

0 MB
4 MB
8 MB
12 MB
16 MB

of a pane defines

Pane 0 | Pane 1 | Pane 2 | Pane 3

where its physical

Window 0

(Virtual)

mapping begins. The

SIZE defines how far it

ATRANS0

ATRANS1

ATRANS2

ATRANS3

extends. A SIZE of 0

base=1M

means no addresses

size=4M

size=0

size=0

size=0

are mapped through

that pane.

Window 0
(Physical)

1 MB
5 MB

Figure 141 shows an example mapping, where the first 4 MB of virtual address space for chip select 0 (virtual address

offsets 0x000000 through 0x3fffff inclusive) map to a 4 MB physical address window starting at a 1 MB offset (physical

address offsets 0x100000 through 0x4fffff inclusive). This mapping could be used for flash that contains a 1 MB

12.14. QSPI memory interface (QMI)
1234
