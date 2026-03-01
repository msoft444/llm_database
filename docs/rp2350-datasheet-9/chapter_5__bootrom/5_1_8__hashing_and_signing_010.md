---
source_pdf: rp2350-datasheet-9.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.1.8. Hashing and signing
pages: 358-359
type: technical_spec
generated_at: 2026-03-01T11:44:31.548839+00:00
---

# 5.1.8. Hashing and signing

5.1.8. Hashing and signing

Any block may be hashed or signed. A hashed block stores the image hash value (see Section 5.9.2.3). At runtime, the

bootrom calculates a hash and compares it to the stored hash to determine if the block is valid. Hashes guard against

5.1. Bootrom concepts
357

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
