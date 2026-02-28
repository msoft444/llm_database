---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 7. Resets
section: 7.3.1. Chip-level reset table
pages: 496-496
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 7.3.1. Chip-level reset table

NOTE
Watchdog scratch registers are not preserved when the watchdog triggers a chip-level reset. However, watchdog
scratch registers are preserved after a system or subsystem reset. For general purpose scratch registers that do not
reset after a chip-level reset, see the POWMAN register block Section 6.4, “Power management (POWMAN) registers”.
7.3. Chip-level resets
Chip-level resets put the entire chip into a default state. These resets are only initiated by hardware events, the
debugger, or a watchdog timeout.
7.3.1. Chip-level reset table
Table 528, “List of chip-level reset causes” shows the components reset by each of the chip-level reset sources. A dash
(—) indicates no change caused by this source.
Table 528. List of
chip-level reset causes
Reset Source
SW-DP
AON Scratch
POWMAN
Power State
Double Tap
Rescue
POR
reset
reset
hard reset
→ P0.0
reset
reset
BOR
reset
reset
hard reset
→ P0.0
reset
reset
EXTERNAL RESET (RUN)
reset
reset
hard reset
→ P0.0
 — 
reset
DEBUGGER RESET REQ
 — 
 — 
hard reset
→ P0.0
 — 
reset
DEBUGGER RESCUE
 — 
 — 
hard reset
→ P0.0
 — 
set
WATCHDOG POWMAN ASYNC RESET
 — 
 — 
hard reset
→ P0.0
 — 
 — 
WATCHDOG POWMAN RESET
 — 
 — 
soft reset
→ P0.0
 — 
 — 
WATCHDOG SWCORE RESET
 — 
 — 
 — 
→ P0.0
 — 
 — 
SWCORE POWERDOWN
 — 
 — 
 — 
→ P0.x
 — 
 — 
GLITCH_DETECTOR
 — 
 — 
 — 
 — 
 — 
 — 
WATCHDOG RESET PSM
 — 
 — 
 — 
 — 
 — 
 — 
All chip-level resets sources in the table also reset the Power-on State Machine (PSM). This asserts all of the system
resets downstream of the PSM. System resets includes low-level chip infrastructure like the system-level clock
generators, as well as the processor cold and warm reset domains.
All chip-level reset sources in the table also reset the system watchdog peripheral. This includes watchdog scratch
registers SCRATCH0 → SCRATCH7.
You can interpret the table columns as follows:
Reset Source
Indicates which of the events listed in Chip-level Reset Sources is responsible for this chip-level reset.
SW-DP
Indicates the SWD Debug Port and the RP-AP (Section 3.5.10, “RP-AP”) are reset.
AON Scratch
Indicates scratch register state in POWMAN SCRATCH0 → SCRATCH7 and BOOT0 → BOOT3 registers is lost.
These registers are always-on, meaning they are preserved across power-down of the switched core domain.
RP2350 Datasheet
7.3. Chip-level resets
495

