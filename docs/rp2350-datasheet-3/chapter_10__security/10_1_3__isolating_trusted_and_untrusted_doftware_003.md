---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 10. Security
section: 10.1.3. Isolating trusted and untrusted doftware
pages: 819-819
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 10.1.3. Isolating trusted and untrusted doftware

RP2350 Datasheet

encrypted binary, the bootrom completes the following steps:

1. Loads the entire encrypted binary into SRAM.

2. Verifies the signature of the decryption stage, then jumps into the decryption stage, comprised of the following

steps:

a. Reads the decryption key stored in OTP (this stage may soft-lock that OTP page until next boot).

b. Decrypts the encrypted binary payload using the decryption key.

c. Calls the chain_image() bootrom API (Section 5.4.8.2) on the decrypted region of SRAM.

3. Verifies the decrypted binary payload in the same manner as it verified the decryption stage, then jumps into the

binary.

The decryption stage is not itself encrypted, but it is signed. Storing the decryption stage in the clear does not present

additional risk because the source code for the decryption stage is open source and highly scrutinised. Without the

decryption key, the encrypted payload cannot be read. Because the key only exists on-device, static analysis of the

encrypted binary cannot recover it.

Resetting the OTP to reopen soft locks also resets the processors. Upon reset, the processors re-run the decryption

stage and re-lock the page with the decryption key. The BOOTDIS register allows the bootrom to detect OTP resets and

disable the watchdog and POWMAN boot vectors. This ensures that the decryption stage is not skipped and the key

remains protected.

NOTE

The decryption stage is deliberately not included in the bootrom, so that it can be updated. The bootrom handles

only public key cryptography, so there is no concern of power analysis attacks, but this reasoning does not apply to

the decryption stage. Power analysis mitigations require iteration as techniques improve.

This scheme supports designs where the decryption key is accessible only to the decryption stage. When the decryption

key is also required at runtime to read additional encrypted flash contents on-demand, processor security features and

OTP page locks can restrict key access to a small subset of trusted code, such as a TF-M Secure Storage service.

In addition to software mitigations provided by the decryption stage, RP2350 supports randomising the frequency

controls of its internal ring oscillator (Section 8.3) to make it more difficult to recover the system clock from power

traces.

Encrypted execute-in-place is not supported in hardware, but the spare 32 MB cached XIP window (Section 4.4.1) can

provide software-defined execute-in-place by trapping cache misses and pinning at the miss address. This may be used

to transparently decrypt data on-demand from external flash.

10.1.3. Isolating trusted and untrusted doftware

In security- or safety-critical applications, access must be limited to those who need it. For example, a JPEG decode

library should not be able to access the core voltage regulator and increase DVDD to 3.3 V (unless you are

decompressing a very large JPEG). The Cortex-M33 processors contain hardware that separates two execution

contexts, known as Secure and Non-secure, and enforces a number of invariants between them, such as:

• Non-secure code cannot access Secure memory
• The Secure context cannot execute Non-secure memory
• Non-secure code cannot directly access peripherals managed by Secure code
• Non-secure code cannot prevent Secure interrupts from being serviced

By making less of your code able to access your most critical hardware and data, you reduce the chance of accidentally

exposing this critical hardware and data to the outside world. For a high-level explanation of how the Cortex-M33

implements this, see Section 10.2. For full details, see Armv8-M Architecture Reference Manual.

10.1. Overview (Arm)
818
