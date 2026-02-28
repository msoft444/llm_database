---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 6. Power
section: 6.5. Power reduction strategies
pages: 489-489
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 6.5. Power reduction strategies

Description
Interrupt Enable
Table 525. INTE
Register
Bits
Description
Type
Reset
31:4
Reserved.
-
-
3
PWRUP_WHILE_WAITING: Source is state.pwrup_while_waiting
RW
0x0
2
STATE_REQ_IGNORED: Source is state.req_ignored
RW
0x0
1
TIMER
RW
0x0
0
VREG_OUTPUT_LOW
RW
0x0
POWMAN: INTF Register
Offset: 0xe8
Description
Interrupt Force
Table 526. INTF
Register
Bits
Description
Type
Reset
31:4
Reserved.
-
-
3
PWRUP_WHILE_WAITING: Source is state.pwrup_while_waiting
RW
0x0
2
STATE_REQ_IGNORED: Source is state.req_ignored
RW
0x0
1
TIMER
RW
0x0
0
VREG_OUTPUT_LOW
RW
0x0
POWMAN: INTS Register
Offset: 0xec
Description
Interrupt status after masking & forcing
Table 527. INTS
Register
Bits
Description
Type
Reset
31:4
Reserved.
-
-
3
PWRUP_WHILE_WAITING: Source is state.pwrup_while_waiting
RO
0x0
2
STATE_REQ_IGNORED: Source is state.req_ignored
RO
0x0
1
TIMER
RO
0x0
0
VREG_OUTPUT_LOW
RO
0x0
6.5. Power reduction strategies
RP2350 retains the SLEEP and DORMANT states for dynamic power control from RP2040. It extends these states by
introducing power domains (Section 6.2.1), which allow power to be removed from various components on chip,
virtually eliminating the leakage currents, and allowing lower power modes to be supported.
RP2350 Datasheet
6.5. Power reduction strategies
488

