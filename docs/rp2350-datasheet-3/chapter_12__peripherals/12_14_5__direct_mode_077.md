---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.14.5. Direct mode
pages: 1236-1236
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 12.14.5. Direct mode

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

12.14.5. Direct mode

In direct mode, the AHB XIP address window is disconnected from the QSPI bus, and the bus is controlled through a

TX/RX FIFO pair, similar to a normal SPI peripheral. In this state, the XIP window becomes inaccessible. Attempting to

access it generates a bus fault. This mode is used for low-level access to the QSPI bus, for example when issuing flash

erase/programming commands, or when accessing QSPI device status registers.

All direct-mode operation is controlled through DIRECT_CSR, with data being exchanged through DIRECT_TX and

DIRECT_RX. To enable direct mode, first set DIRECT_CSR.EN, and then poll for DIRECT_CSR.BUSY to go low to ensure

that any in-progress XIP transfer at the point direct mode was enabled has completed.

Direct mode has its own clock divisor and RX sampling delay, configured by DIRECT_CSR.CLKDIV and

DIRECT_CSR.RXDELAY. These are separate from the per-window settings configured in M0_TIMING/M1_TIMING,

because serial commands used for control purposes may have different frequency limits than data accesses used for

execute-in-place.

For each push to DIRECT_TX, QMI will issue 8 or 16 bits of FIFO data to the QSPI bus. Optionally, the same number of

bits are simultaneously sampled and returned in DIRECT_RX. The clock is initially low, and data is always captured on

the rising edge of SCK, transitioning on the subsequent falling edge.

After pushing to DIRECT_TX, DIRECT_CSR.BUSY will go high, and remain high until all direct-mode activity has

completed. This works even if no RX data is returned, so is more reliable than polling the RX FIFO status. The BUSY flag

12.14. QSPI memory interface (QMI)
1235
