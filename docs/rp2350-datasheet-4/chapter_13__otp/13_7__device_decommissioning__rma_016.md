---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 13. OTP
section: 13.7. Device decommissioning (RMA)
pages: 1280-1280
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 13.7. Device decommissioning (RMA)

13.7. Device decommissioning (RMA)

Decommissioning refers to destroying a device’s sensitive contents and restoring some test or debug functionality

when a device reaches the end of its security lifecycle. The OTP hardware can’t actually destroy user data without

circumventing write protection in some way. Instead, decommissioning is implemented with the RMA flag, which

modifies devices in the following ways:

• re-enables factory test JTAG which is otherwise disabled by the secure boot critical flag
• makes pages 3 through 61 inaccessible

The RMA flag doesn’t change permissions for page 0 (manufacturing data), pages 1 and 2 (boot configuration), page 61

(OTP access keys), or pages 62 and 63 (locks).

The RMA flag is encoded in a spare bit of the page 63 lock word. This lock word would otherwise be unused, since page

63 is one of the lock pages; consequently, it is not protected by a lock word. Instead, each lock word protects itself.

Like all other lock words, the page 63 lock word is protected by its own locks, which means it can be hard- and soft-

locked to prevent the RMA flag being set. Locking the RMA flag makes it impossible to re-enable the factory JTAG

interface if any of CRIT1.SECURE_BOOT_ENABLE, CRIT1.DEBUG_DISABLE or CRIT1.SECURE_DEBUG_DISABLE is set.

This makes it impossible for Raspberry Pi to re-test such devices if they are returned for fault analysis.

IMPORTANT

Setting the RMA flag does not destroy OTP contents, it merely renders it inaccessible. The design intent is for this to

be irreversible, but hardware is never perfect. This is something the user’s threat model must account for when

programming the RMA flag on devices with sensitive OTP contents — for example, by personalising per-device OTP

secrets to avoid class breaks if an attacker is able to retrieve the keys.

13.8. Imaging Vulnerability

The RP2350 OTP is intended to store boot key fingerprints and boot decryption keys. The ability to protect encrypted

contents in external flash storage depends on the ability to protect the OTP contents from unauthorised or external

reads. The OTP uses antifuse bit cells, which store data as a charge, similar to a flash bit cell. They do not make use of

a physical structural change as used in a traditional fuse cell. This makes them resistant to many imaging techniques,

such as optical and scanning electron microscopy. However antifuse cells can be imaged using a novel technique

called passive voltage contrast (PVC), using a focused ion beam (FIB) device.

PVC Whitepaper

For more information on passive voltage contrast imaging, read the whitepaper by IOActive:

https://www.ioactive.com/wp-content/uploads/2025/01/IOActive-RP2350HackingChallenge.pdf

This process involves decapsulating the die. Therefore physical access to the device is a strict requirement, and there is

a moderate chance of destroying the die without being able to recover its OTP contents.
