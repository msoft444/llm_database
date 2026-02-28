---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 10. Security
section: 10.1.1. Secure boot
pages: 817-817
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 10.1.1. Secure boot

RP2350 Datasheet

Chapter 10. Security

This chapter describes the RP2350 security model and the hardware that implements it. This chapter contains two

separate overviews: one for Arm, and one for RISC-V. The architectures have distinct security features and levels of

bootrom support.

10.1. Overview (Arm)

RP2350 provides hardware and bootrom security features for three purposes:

1. Prevent unauthorised code from running on the device

2. Prevent unauthorised reading of user code and data

3. Isolate trusted and untrusted software, running concurrently on the device, from one another

Point 1 is referred to in this datasheet as secure boot. Secure boot is a prerequisite to points two and three because

running unauthorised code on the device allows that code to access device internals. The bootrom secure boot

implementation and related hardware security features implement the root of trust for secure RP2350 applications;

bootrom contents are fixed at design time and immutable.

Point 2 is referred to in this datasheet as encrypted boot. Encrypted boot is an additional layer of protection which

makes it more difficult to clone devices, or dump and reverse-engineer device firmware. Encrypted boot is implemented

using a signed decryption stage prepended to a binary as a post-build step. Encrypted boot stores decryption keys in on-

device OTP memory, which can be locked down after use.

Point 3 allows applications to enforce internal security boundaries such that one part of an application being

compromised does not allow access to critical hardware, such as the voltage regulator or protected OTP storage used

for cryptographic keys.

Hardware features such as the glitch detector and redundancy coprocessor mitigate common classes of fault injection

attacks and help maintain boot integrity, even when an attacker has physical access.

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
