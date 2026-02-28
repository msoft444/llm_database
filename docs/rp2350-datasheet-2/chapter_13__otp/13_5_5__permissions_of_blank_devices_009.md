---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 13. OTP
section: 13.5.5. Permissions of blank devices
pages: 1277-1277
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 13.5.5. Permissions of blank devices

![Page 1277 figure](images/fig_p1277.png)

RP2350 Datasheet

Non-secure debug access key, respectively. See Section 3.5.9.2 for information on how the debug keys affect external

debug access.

You might use OTP access keys if a bootloader contains OTP configuration that needs to be Secure-writable only to the

board owner, not to general Secure software on the device.

13.5.3. Lock encoding in OTP

Page locks are encoded as a 16-bit value. This value is stored as a pair of triple-redundant bytes, each byte occupying a

24-bit OTP row.

The lock halfword is encoded as follows:

| Bits | Purpose |
| --- | --- |
| 2:0 | Write key index, or 0 if no write key |
| 5:3 | Read key index, or 0 if no read key |
| 6 | No-key state, 0=Read-only 1=Inaccessible |
| 7 | Reserved |
| 9:8 | Secure lock state (thermometer code 0 → 2) |
| 11:10 | Non-secure lock state (thermometer code 0 → 2) |
| 13:12 | PicoBoot lock state (thermometer code 0 → 2) or software-defined use if PicoBoot OTP is disabled |
| 15:12 | Reserved |

13.5.4. Special pages

The following pages require special case handling in their lock checks:

• The lock word region itself (pages 62 and 63)

◦Lock words are always world-readable

◦Lock words are writable by Secure code if the lock word itself permits Secure writes

◦Consequently, lock words 62 and 63 are considered "spare", since they do not protect pages 62 and 63; the

page 63 lock word is repurposed for the RMA flag
• The hardware access key page (page 61)

◦Contains OTP access keys and debug access keys

◦Each key also has a valid bit (rbit)

◦Page 61 (key page) has all of the usual protections from the page 61 lock word

◦If a key’s valid bit is set, that key is inaccessible; the converse is not necessarily true

Page 0, known as the chip info page, is not a special page. Raspberry Pi sets page 0 to read-only during factory test,

after writing chip identification and calibration values.

13.5.5. Permissions of blank devices

Each RP2350 device has some information programmed during manufacturing test. At this time, a small number of

hard page lock bits are also programmed:

13.5. Page locks
1276
