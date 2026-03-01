---
source_pdf: rp2350-datasheet-5.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.1.10. Packaged binaries
pages: 359-360
type: technical_spec
generated_at: 2026-03-01T04:13:03.328193+00:00
---

# 5.1.10. Packaged binaries

5.1.10. Packaged binaries

As described in Section 5.1.9, signed binaries in flash on a secured RP2350 are commonly loaded from flash into RAM,

go through signature verification in RAM, and then execute from the verified version in RAM.

A packaged binary is a binary stored in flash that runs entirely from RAM. The binary is likely compiled to run from RAM

as a RAM-only binary (unfortunately named no_flash in SDK parlance), but subsequently post-processed for flash

5.1. Bootrom concepts
358

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
