---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 10. Security
section: 10.1.2. Encrypted boot
pages: 818-818
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 10.1.2. Encrypted boot

10.1.2. Encrypted boot

RP2350 contains 8 kB of OTP, which can be protected at 128-byte page granularity. This protection comes in the

following forms:

• hard locks, which permanently revoke read or write access by Secure or Non-secure code
• soft locks, which revoke permissions only until the next reset of the OTP block.

Encrypted boot stores decryption keys in OTP, and protects the keys from later boot stages using soft locks.

RP2350 supports loading encrypted binaries from external flash into SRAM, which can then decrypt their own contents

in-place. Many implementations are possible, but as a concrete example, this section describes the flash-resident binary

encryption support provided by the SDK and picotool.

1. First, the developer should process a plain SRAM binary into an encrypted binary. To encrypt your binary, the SDK

completes the following steps after a build:

a. Sign the payload binary using the boot private key, if you didn’t already do this during the build.

b. Encrypt the payload binary using the encryption key (not the private key).

c. Append a small decryption stage to the binary that contains a modified copy of the payload’s IMAGE_DEF (the

original is unreadable, as it is encrypted).

d. Sign the decryption stage together with the encrypted contents, using the boot private key.

Encrypted binaries boot as packaged RAM binaries (Section 5.1.10), decrypting themselves in-place. To boot an

10.1. Overview (Arm)
817
