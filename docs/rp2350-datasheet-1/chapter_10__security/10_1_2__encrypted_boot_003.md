---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 10. Security
section: 10.1.2. Encrypted boot
pages: 818-818
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 10.1.2. Encrypted boot

counter stored in OTP. The bootrom refuses to boot images with rollback versions lower than the OTP counter number,
and automatically increments the OTP counter upon booting a higher version. This is useful if older binaries have known
vulnerabilities, as installing a newer version automatically revokes the ability to downgrade to older versions.
Incrementing the major and minor versions allows you to express a preference for newer, higher binary versions without
blocking execution of older, lower-versioned binaries. For more discussion of bootrom anti-rollback support, see Section
5.1.11.
RP2350 can boot from any of the following sources:
• Directly on external flash via execute-in-place (XIP)
• Loading into SRAM from external flash
• Loading into SRAM from user-specified OTP contents
• Loading into SRAM via USB or other serial bootloader
• Loading into SRAM via debugger
RP2350 enforces signatures on all of these boot media, with the exception of the debugger, when an external host has
control of RP2350’s processors and can completely skip execution of the bootrom. Disabling debug is part of the
secure boot enable procedure outlined in Section 10.5.
Although signatures can be enforced on a flash execute-in-place binary, we do not recommend it. With this boot media,
flash contents can change between checks and execution. For example, an attacker could emulate a QSPI device using
an FPGA or another microcontroller. Instead, load your complete application into SRAM and verify it in-place before
execution. RP2350 has sufficient SRAM capacity to do this with most applications.
Pure-software secure boot implementations are susceptible to fault injection attacks when an attacker has physical
access to the device, as is often the case for embedded hardware. Our very own Pico is a popular tool for voltage fault
injection. Instead of potentially booting an unauthorised binary, the RP2350 glitch detectors (Section 10.9) and
redundancy coprocessor (Section 3.6.3) mitigate fault injection attacks by detecting out-of-envelope operation and
bringing the system to a safe halt. To enable the glitch detectors, set the CRIT1.GLITCH_DETECTOR_ENABLE OTP flag.
The redundancy coprocessor is always used by the bootrom.
To learn more about how to enable secure boot on a blank RP2350 device, see Section 10.5.
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
RP2350 Datasheet
10.1. Overview (Arm)
817

