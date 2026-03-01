---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.1.10. Packaged binaries
pages: 359-359
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 5.1.10. Packaged binaries

RP2350 Datasheet

corruption of an image, but do not provide any security guarantees.

On a secured RP2350, a hash is not sufficient for an image to be considered valid. All images must have a signature: a

hash encrypted by a private key, plus metadata (also covered by the hash) describing how the hash was generated. This

signature is stored as part of an IMAGE_DEF block. An image with a signature in its IMAGE_DEF block is called a signed

image.

NOTE

For background on signatures and boot keys, see the introduction to secure boot in the security chapter (Section

10.1.1).

To verify a signed image, the bootrom decrypts the hash stored in the signature using a secp256k1 public key. The

bootrom also computes its own hash of the image and compares its measured hash value with the one in the signature.

The public key is also stored in the block via a SIGNATURE item (see Section 5.9.2.4): this key’s (SHA-256) hash must

match one of the boot key hashes stored in OTP locations BOOTKEY0_0 onwards. Up to four public keys can be

registered in OTP, with the count defined by BOOT_FLAGS1.KEY_VALID and BOOT_FLAGS1.KEY_INVALID. A hash of a

key is also referred to as a key fingerprint.

The data to be hashed is defined by a HASH_DEF item (see Section 5.9.2.2), which indicates the type of hash. It also

indicates how much of the block itself is to be hashed. For a signed block, the hash must contain all contents of the

block up to the final SIGNATURE item.

To be useful your hash or signature must cover actual image data in addition to the metadata stored in the block. The

block’s load map item specifies which data the bootrom hashes during hash or signature verification.

The 
above 
discussion 
mostly 
applies 
to 
IMAGE_DEFs. 
On 
a 
secured 
RP2350 
with 
the

BOOT_FLAGS0.SECURE_PARTITION_TABLE flag set, the bootrom also enforces signatures on PARTITION_TABLEs.

5.1.9. Load maps

A load map describes regions of the binary and what to do with them before the bootrom runs the binary.

The load map supports:

• Copying portions of the binary from flash to RAM (or to the XIP cache)
• Clearing parts of RAM (either .bss clear, or erasing uninitialised memory during secure boot)
• Defining what parts of the binary are included in a hash or signature
• Preventing the flushing of the XIP cache when to keep loaded lines pinned up to the point the binary starts

For full details on the LOAD_MAP item type of IMAGE_DEF blocks, see Section 5.9.3.2.

When booting a signed binary from flash, it is desirable to load the signed data and code into RAM before checking the

signature and subsequently executing it. Otherwise, an adversary could replace the flash device in between the

signature check and execution, subverting the check. For this reason, the load map also serves as a convenient

description of what to include in a hash or signature. The load map itself is covered by the hash or signature, and the

entire metadata block is loaded into RAM before processing, so it is not itself subject to this time-of-check versus time-

of-use concern.

5.1.10. Packaged binaries

As described in Section 5.1.9, signed binaries in flash on a secured RP2350 are commonly loaded from flash into RAM,

go through signature verification in RAM, and then execute from the verified version in RAM.

A packaged binary is a binary stored in flash that runs entirely from RAM. The binary is likely compiled to run from RAM

as a RAM-only binary (unfortunately named no_flash in SDK parlance), but subsequently post-processed for flash

5.1. Bootrom concepts
358
