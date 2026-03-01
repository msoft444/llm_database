---
source_pdf: rp2350-datasheet-6.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.14.4. Address translation
pages: 1235-1236
type: technical_spec
generated_at: 2026-03-01T05:39:21.783275+00:00
---

# 12.14.4. Address translation

![Page 1235 figure](images/fig_p1235.png)

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

12.14. QSPI memory interface (QMI)
1234

RP2350 Datasheet

bootloader application followed by a 4 MB user application. Ideally, the user application should not be aware of the flash

layout defined by the bootloader; that way, the same application can run under different bootloader implementations.

The virtual-to-physical mapping solves this problem by making the storage location of the user application (starting at

1 MB) independent of the address it appears at in the system address space (starting at 0 MB).

12.14.4.1. Bootrom support for address translation

The bootrom can automatically configure address translation at boot time, so that a binary stored at some arbitrary

location in physical flash storage can appear at a runtime flash address of 0.

This is done automatically when the booted image is inside of a flash partition (Section 5.1.2), and can be adjusted

manually based on a rolling window delta specified in the IMAGE_DEF of the launched executable (Section 5.1.4).

The bootrom source code and bootrom documentation often refers to the QMI ATRANS mapping as "rolling windows", due

to the modulo address wrapping on 16 MB boundaries — see Section 5.1.19.

12.14.4.2. Translation and the XIP cache

The QMI address translation is performed downstream of the system XIP cache (Section 4.4.1). Therefore, the XIP

cache is a virtual cache with respect to this translation, because the address translation performed inside QMI is

opaque to the XIP cache.

Consequently, changes to the QMI address translation necessitate a flush of the XIP cache. From the cache’s point of

view, the translation change has moved QMI memory contents around in the cache’s downstream address space in a

way that is incoherent with the cache contents, so a flush is required to restore coherence. At a minimum, any virtual

address whose ATRANSx register (ATRANS0 through ATRANS7) has been modified, and which may be allocated in the

cache in either the clean or the dirty state, must be flushed. It may be simplest to flush the entire cache.

QMI’s address mapping creates another hazard: the same physical address may map to multiple virtual addresses, and

therefore may be allocated multiple times in the XIP cache. When you write to a physical address through a cached

virtual address alias, the XIP cache does not propagate the change to other aliases. To avoid this issue, do not allow

multiple aliases of the same writable physical address at the same instant. Aliasing read-only memory is usually safe.

Aliases that exist at different points in time (for example, across an RTOS context switch boundary) can be kept

coherent with appropriate cleaning and flushing when the translation is changed.
