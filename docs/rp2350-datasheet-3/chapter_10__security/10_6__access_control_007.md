---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 10. Security
section: 10.6. Access control
pages: 823-823
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 10.6. Access control

RP2350 Datasheet

User-mode RWX permission on the ROM. These are assigned region numbers 8 through 10. Because lower-numbered

regions always take precedence, any dynamically-configured region can override these hardwired regions.

There are many more peripherals than PMP regions. In typical use-cases, the programmer assigns these peripherals

blanket U-mode RW permissions. Because hardwired regions are much cheaper than dynamically-configured regions, it

was more efficient to use hardwired regions. These regions are included because the peripherals are expected to be

assigned using ACCESSCTRL, rather than PMP. The hardwired regions play a similar role to the Exempt regions in the

RP2350 Cortex-M IDAU.

Together with the ACCESSCTRL filters, these PMP regions are an effective mechanism for partitioning between

addresses accessible from U-mode and addresses not accessible from U-mode. Hazard3 includes one custom PMP

feature, the PMPCFGM0 register, which allows the PMP to set M-mode permissions as well as U-mode without locking.

This is useful for preventing accidental (but not deliberate) access to a memory region.

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

10.6. Access control

The access control registers (ACCESSCTRL) define permissions required to access GPIOs and bus endpoints such as

peripherals and memory devices.

For each bus endpoint (for example, PIO0), a bus access control register such as PIO0 controls which AHB5 managers

can access it, and at which bus security levels. This register has further implications, such as access to the RESETS

controls for that block. For a full explanation of the bus access control registers, see Section 10.6.2.

For each GPIO, including the QSPI and USB DP/DM pins, a bit in the GPIO_NSMASK0 and GPIO_NSMASK1 register can

be set to make that GPIO accessible to both the Secure and Non-secure domains, or clear to make it Secure-only. This

has system-wide implications, controlling:

• GPIO visibility to the Non-secure SIO

10.5. Secure boot enable procedure
822
