---
source_pdf: rp2350-datasheet-10.pdf
repository: llm_database
chapter: Chapter 13. OTP
section: 13.7. Device decommissioning (RMA)
pages: 1280-1280
type: technical_spec
generated_at: 2026-03-01T12:20:14.494106+00:00
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
