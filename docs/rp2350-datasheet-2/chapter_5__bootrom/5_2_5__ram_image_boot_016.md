---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.2.5. RAM image boot
pages: 373-373
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 5.2.5. RAM image boot

RP2350 Datasheet

zeroes SCRATCH4 before transferring control, so that the behaviour does not persist over subsequent reboots.

Watchdog boot can also be used to select the bootrom’s special one-shot boot modes, described in Section 5.2.4.1. The

term one-shot refers to the fact these only affect the next boot (and not subsequent ones) due to the bootrom clearing

SCRATCH4 each boot. These boot types are encoded by setting a special entry point (pc) value of 0xb007c0d3, which is

otherwise not a valid entry address, and then setting the boot type in the stack pointer (sp) value. Section 5.2.4.1 lists

the supported values.

The watchdog boot vector is permitted to return. The boot path continues as normal when it returns: use this to perform

any additional setup required for the boot path, such as issuing additional commands to an external QSPI device. On

RISC-V the vector is permitted to use its own global pointer (gp) value, as the bootrom only uses gp during USB boot,

which installs its own value.

With the exception of the magic boot type entry point (0xb007c0d3), the vector entry point pc must have the LSB set on

Arm (the Thumb bit) and clear on RISC-V. If this condition is not met, the bootrom assumes you have passed a RISC-V

function pointer to an Arm processor (or vice versa) and hangs the core rather than continuing.

5.2.4.1. Special watchdog boot types

The magic entry point 0xb007c0d3 indicates a special one-shot boot type, identified by the stack pointer value:

BOOTSEL

Selected by sp = 2. Boot into BOOTSEL mode. This will be either UART or USB boot depending on whether QSPI SD1

is driven high (default pull-down selects USB boot). See Section 5.2.8 for more details.

RAM_IMAGE

Selected by sp = 3. Boot into an image stored in SRAM or XIP SRAM. BOOTSEL mode uses this to request execution

of an image it loaded into RAM before rebooting. See Section 5.2.5 for more details.

FLASH_UPDATE

Selected by sp = 4. BOOTSEL selects this mode when rebooting following a flash download. Changes some flash

boot behaviour, such as allowing older versions to boot in preference to newer ones. See Section 5.1.16 for more

details.

Parameters to the one-shot boot type are passed in:

• SCRATCH2: Parameter 0
• SCRATCH3: Parameter 1

These directly correspond to the p0 and p1 boot parameters passed into the reboot() API. For example, on a RAM_IMAGE

boot, this specifies the base and size of the RAM region to be searched for a valid IMAGE_DEF. See the API listing in

Section 5.4.8.24 for more details. When not performing one of the listed boot types, SCRATCH2 and SCRATCH3 remain

free for arbitrary user values, and the bootrom doesn’t modify or interpret their contents.

5.2.5. RAM image boot

The bootrom is directed (through values in the watchdog registers) to boot into an image in SRAM or XIP SRAM. The

two parameters indicate the start and size of the region to search for a block loop containing a valid (and correctly

signed if necessary) IMAGE_DEF. These are passed as parameter 0/1, in watchdog scratch 2/3.

If the image to be booted is contained in XIP SRAM, the XIP SRAM must be pinned in place by the bootrom prior to

launch. For this reason, if you are using XIP SRAM for your binaries, you must add a special entry to the LOAD_MAP item

(see Section 5.9.3.2).

5.2. Processor-controlled boot sequence
372
