---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 10. Security
section: 10.1.1. Secure boot
pages: 817-817
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 10.1.1. Secure boot

10.1.1. Secure boot

You can permanently alter blank RP2350 devices to restrict code execution to only your own code. With further

alteration, you can revoke the ability to run older software versions.

The RP2350 bootrom uses a cryptographic signature to distinguish authentic from inauthentic binaries. A signature is a

hash of the binary signed with the user’s private key. You can include signatures in binary images compiled for RP2350

devices. Signatures use the SHA-256 hash algorithm and secp256k1 ECDSA elliptic curve cipher to authenticate

binaries. The bootrom authenticates binaries using the following steps:

1. Calculate a SHA-256 hash using image code and data when loading the binary.

2. Verify the image’s signature using the user’s public key, which is also stored in the image.

3. Check the included, verified signature (from step 2) against the calculated SHA-256 hash value for the binary (from

step 1).

4. Check the image’s public key against a SHA-256 key fingerprint stored in OTP.

If both checks succeed, the bootrom assumes someone in possession of the private key registered by an OTP public

key fingerprint calculated the same SHA-256 hash. Based on the properties of hash functions, the bootrom assumes

that the binary contents have not been altered since the signature was generated. This proves that this is an authentic

binary signed by the owner of the private key, so the bootrom will entertain the idea of running the binary.

The image may also have an anti-rollback version number (rollback.major.minor) that the bootrom checks against a

10.1. Overview (Arm)
816
