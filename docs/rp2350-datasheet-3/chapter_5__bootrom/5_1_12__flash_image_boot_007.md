---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.1.12. Flash image boot
pages: 360-360
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 5.1.12. Flash image boot

RP2350 Datasheet

residence. The bootrom unpackages the binary into RAM before execution.

As part of the packaging process, tooling like picotool adds a LOAD_MAP that tells the bootrom which parts of the flash-

resident image it must load into RAM, and where to put them. This tooling may also hash or sign the binary in the same

step. In this case, the bootrom hashes the data it loads as it unpackages the binary, as well as relevant metadata such

as the LOAD_MAP itself. The bootrom compares the resulting hash to the precomputed hash or signature in the IMAGE_DEF to

verify the unpackaged contents in RAM before running those contents.

Compare this with RP2040, where a flash-resident binary which executes from RAM (a copy_to_ram binary in SDK

parlance) must begin by executing from flash, then copy itself to RAM before continuing from there. In the RP2040 case,

the loader itself (or rather the SDK crt0) executes in-place in flash to perform the copy. This makes it impossible to

perform any trustworthy level of verification, because the loader itself executes in untrusted memory.

5.1.11. Anti-rollback protection

Anti-rollback on a secured RP2350 prevents booting an older binary which may have known vulnerabilities. It prevents

this even if the binary is correctly signed and meets all other requirements for bootability.

Full IMAGE_DEF version information is of the form (rollback).major.minor, where the rollback part is optional. If a rollback

version is present, it is accompanied by a list of OTP rows whose ordered values are used to form a thermometer of

bits indicating the minimum rollback version that may run on the device.

A thermometer code is a base-1 (unary) number where the integer value is one plus the index of the most-significant set

bit. For example, the bit strings 00001111, 00001001, and 00001000 all encode a value of four, and the all-zeroes bit pattern

encodes a value of zero. The bootrom uses this encoding because:

• it allows OTP rows containing counters to be incremented, and
• it does not allow them to be decremented

On a secured RP2350, the bootrom compares the rollback version of the IMAGE_DEF against the thermometer-coded

minimum rollback version stored in OTP. If the IMAGE_DEF value is lower, the bootrom refuses to boot the image.

The IMAGE_DEF rollback version is covered by the image’s signature, thus cannot be modified by an adversary who does

not know the signing key. The list of OTP rows which define the chip’s minimum rollback version is also stored in the

program image, and also covered by the image signature.

The list of OTP rows in the IMAGE_DEF must always have at least one bit spare beyond the IMAGE_DEF's rollback version

(enforced by picotool). As a result, older binaries always contain enough information for the bootrom to detect that the

chip’s minimum rollback version has been incremented past the rollback version in the IMAGE_DEF. You can append more

rows to the list on newer binaries to accommodate higher rollback versions without ambiguity.

When an executable image with a non-zero rollback version is successfully booted, its rollback version is written to the

OTP thermometer. The BOOT_FLAGS0.ROLLBACK_REQUIRED flag may be used to require an IMAGE_DEF have a rollback

version on a secured RP2350. This flag is set automatically when updating the rollback version in OTP.

NOTE

An IMAGE_DEF with a rollback version of 0 will not automatically set the BOOT_FLAGS0.ROLLBACK_REQUIRED flag, so

it is recommended that the minimum rollback version used is 1, unless the BOOT_FLAGS0.ROLLBACK_REQUIRED

flag is manually set during provisioning.

5.1.12. Flash image boot

RP2350 is designed primarily to run code from a QSPI flash device, either in-package or soldered separately to the

circuit board. Code runs either in-place in flash, or in SRAM after being loaded from flash. Flash boot is the process of

discovering that code and preparing to run it. Flash image boot uses a program binary stored directly in flash rather

than in a flash partition. Flash image boot requires the bootrom to discover a block loop starting within the first 4 kB of

flash which contains a valid IMAGE_DEF (and no PARTITION_TABLE).

5.1. Bootrom concepts
359
