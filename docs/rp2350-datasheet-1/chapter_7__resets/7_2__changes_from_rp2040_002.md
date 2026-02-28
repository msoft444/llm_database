---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 7. Resets
section: 7.2. Changes from RP2040
pages: 495-495
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 7.2. Changes from RP2040

Chapter 7. Resets
7.1. Overview
Resets are divided into three categories, each of which applies to a subset of RP2350:
Chip-level resets
apply to the entire chip. Used to put the entire chip into a default state. These are initiated by hardware events, the
watchdog, or the debugger. When all chip level resets are de-asserted, the system resets are released and the
processors boot.
System resets
apply to components essential to processor operation. System components have interdependencies, therefore their
resets are de-asserted in sequence by the Power-on State Machine (PSM). The full PSM sequence is triggered by
deassertion of chip-level resets. A full or partial sequence can be triggered by the watchdog or debugger. The
sequence culminates in processor boot.
Subsystem resets
apply to components not essential for operation of the processors. The resets can be independently asserted by
writing to the RESETS registers and de-asserted by software, the watchdog, or the debugger.
The watchdog can be programmed to trigger any of the above categories.
7.2. Changes from RP2040
RP2350 retains all RP2040 chip-level reset features.
RP2350 adds the following features:
• new chip reset sources:
◦glitch detector
◦watchdog
◦debugger
• new destinations:
◦new power management components
RP2350 makes the following modifications to existing features:
• Modified the CHIP_RESET register, which records the source of the last chip level reset. In RP2040, CHIP_RESET was
stored in the LDO_POR register block. In RP2350, CHIP_RESET was extended and moved to the POWMAN register block,
which is in the new always-on power domain (AON).
• Renamed the brownout reset (BOR) registers to brownout detect (BOD), added functionality, and moved them to the
new POWlMAN register block.
• Added more system reset stages. To support this, added additional Power-on State Machine fields and rearranged
the existing fields.
• Added additional RESETS registers and rearranged the existing fields.
• Extended watchdog options to enable triggers for new resets.
RP2350 Datasheet
7.1. Overview
494

