---
source_pdf: rp2350-datasheet-8.pdf
repository: llm_database
chapter: Chapter 10. Security
section: 10.5. Secure boot enable procedure
pages: 823-823
type: technical_spec
generated_at: 2026-03-01T08:45:43.944040+00:00
---

# 10.5. Secure boot enable procedure

10.5. Secure boot enable procedure

To enable secure boot:

1. Program at least one public key fingerprint into OTP, starting at BOOTKEY0_0.

2. Mark programmed keys as valid by programming BOOT_FLAGS1.KEY_VALID.

3. Optionally, mark unused keys as invalid by programming BOOT_FLAGS1.KEY_INVALID — this is recommended to

prevent a malicious actor installing their own boot keys at a later date.

◦KEY_INVALID takes precedence over KEY_VALID, which prevents more keys from being added later.

◦Program KEY_INVALID with additional bits to revoke keys at a later time.

4. Disable debugging by programming CRIT1.DEBUG_DISABLE, CRIT1.SECURE_DEBUG_DISABLE, or installing a

debug key (Section 3.5.9.2).

5. Optionally, enable the glitch detector (Section 10.9) by programming CRIT1.GLITCH_DETECTOR_ENABLE and

setting the desired sensitivity in CRIT1.GLITCH_DETECTOR_SENS.

6. Disable unused boot options such as USB and UART boot in BOOT_FLAGS0.

7. Enable secure boot, by programming CRIT1.SECURE_BOOT_ENABLE.

WARNING

This procedure is irreversible. Before programming, ensure that you are using the correct public key, correctly

hashed. picotool supports programming keys into OTP from standard PEM files, performing the fingerprint hashing

automatically. Programming the wrong key will make it impossible to run code on your device.
