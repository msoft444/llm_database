---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 8. Clocks
section: 8.6.5. List of Registers
pages: 584-587
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 8.6.5. List of Registers

50     reset_unreset_block_num_wait_blocking(PLL_RESET_NUM(pll));
51 
52     // Load VCO-related dividers before starting VCO
53     pll->cs = refdiv;
54     pll->fbdiv_int = fbdiv;
55 
56     // Turn on PLL
57     uint32_t power = PLL_PWR_PD_BITS | // Main power
58                      PLL_PWR_VCOPD_BITS; // VCO Power
59 
60     hw_clear_bits(&pll->pwr, power);
61 
62     // Wait for PLL to lock
63     while (!(pll->cs & PLL_CS_LOCK_BITS)) tight_loop_contents();
64 
65     // Set up post dividers
66     pll->prim = pdiv;
67 
68     // Turn on post divider
69     hw_clear_bits(&pll->pwr, PLL_PWR_POSTDIVPD_BITS);
The VCO turns on first, followed by the post dividers, so the PLL does not output a dirty clock while waiting for the VCO
to lock.
8.6.5. List of Registers
The PLL_SYS and PLL_USB registers start at base addresses of 0x40050000 and 0x40058000 respectively (defined as
PLL_SYS_BASE and PLL_USB_BASE in SDK).
Table 636. List of PLL
registers
Offset
Name
Info
0x00
CS
Control and Status
0x04
PWR
Controls the PLL power modes.
0x08
FBDIV_INT
Feedback divisor
0x0c
PRIM
Controls the PLL post dividers for the primary output
0x10
INTR
Raw Interrupts
0x14
INTE
Interrupt Enable
0x18
INTF
Interrupt Force
0x1c
INTS
Interrupt status after masking & forcing
PLL: CS Register
Offset: 0x00
Description
Control and Status
GENERAL CONSTRAINTS:
Reference clock frequency min=5MHz, max=800MHz
Feedback divider min=16, max=320
VCO frequency min=400MHz, max=1600MHz
Table 637. CS Register
Bits
Description
Type
Reset
31
LOCK: PLL is locked
RO
0x0
RP2350 Datasheet
8.6. PLL
583


Bits
Description
Type
Reset
30
LOCK_N: PLL is not locked
Ideally this is cleared when PLL lock is seen and this should never normally be
set
WC
0x0
29:9
Reserved.
-
-
8
BYPASS: Passes the reference clock to the output instead of the divided VCO.
The VCO continues to run so the user can switch between the reference clock
and the divided VCO but the output will glitch when doing so.
RW
0x0
7:6
Reserved.
-
-
5:0
REFDIV: Divides the PLL input reference clock.
Behaviour is undefined for div=0.
PLL output will be unpredictable during refdiv changes, wait for lock=1 before
using it.
RW
0x01
PLL: PWR Register
Offset: 0x04
Description
Controls the PLL power modes.
Table 638. PWR
Register
Bits
Description
Type
Reset
31:6
Reserved.
-
-
5
VCOPD: PLL VCO powerdown
To save power set high when PLL output not required or bypass=1.
RW
0x1
4
Reserved.
-
-
3
POSTDIVPD: PLL post divider powerdown
To save power set high when PLL output not required or bypass=1.
RW
0x1
2
DSMPD: PLL DSM powerdown
Nothing is achieved by setting this low.
RW
0x1
1
Reserved.
-
-
0
PD: PLL powerdown
To save power set high when PLL output not required.
RW
0x1
PLL: FBDIV_INT Register
Offset: 0x08
Description
Feedback divisor
(note: this PLL does not support fractional division)
Table 639. FBDIV_INT
Register
Bits
Description
Type
Reset
31:12
Reserved.
-
-
11:0
see ctrl reg description for constraints
RW
0x000
PLL: PRIM Register
Offset: 0x0c
RP2350 Datasheet
8.6. PLL
584


Description
Controls the PLL post dividers for the primary output
(note: this PLL does not have a secondary output)
the primary output is driven from VCO divided by postdiv1*postdiv2
Table 640. PRIM
Register
Bits
Description
Type
Reset
31:19
Reserved.
-
-
18:16
POSTDIV1: divide by 1-7
RW
0x7
15
Reserved.
-
-
14:12
POSTDIV2: divide by 1-7
RW
0x7
11:0
Reserved.
-
-
PLL: INTR Register
Offset: 0x10
Description
Raw Interrupts
Table 641. INTR
Register
Bits
Description
Type
Reset
31:1
Reserved.
-
-
0
LOCK_N_STICKY
WC
0x0
PLL: INTE Register
Offset: 0x14
Description
Interrupt Enable
Table 642. INTE
Register
Bits
Description
Type
Reset
31:1
Reserved.
-
-
0
LOCK_N_STICKY
RW
0x0
PLL: INTF Register
Offset: 0x18
Description
Interrupt Force
Table 643. INTF
Register
Bits
Description
Type
Reset
31:1
Reserved.
-
-
0
LOCK_N_STICKY
RW
0x0
PLL: INTS Register
Offset: 0x1c
Description
Interrupt status after masking & forcing
RP2350 Datasheet
8.6. PLL
585


Table 644. INTS
Register
Bits
Description
Type
Reset
31:1
Reserved.
-
-
0
LOCK_N_STICKY
RO
0x0
RP2350 Datasheet
8.6. PLL
586

