---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.9.6. Programmer’s model
pages: 1195-1196
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 12.9.6. Programmer’s model

12.9.4. Control watchdog reset levels
To control the level of reset triggered by a watchdog event, use the registers outside the watchdog register block:
• POWMAN_WATCHDOG allows the watchdog to trigger chip level resets
• PSM_WDSEL allows the watchdog to trigger system resets by running a full or partial PSM sequence (Power-on State
Machine)
• RESETS_WDSEL allows the watchdog to trigger subsystem resets
These are described in the Resets section, see Chapter 7.
12.9.5. Scratch registers
The watchdog contains eight 32-bit scratch registers that can store information between soft resets of the chip. The
scratch registers reset when:
• the watchdog is used to to trigger a chip level reset
• a rst_n_run event occurs, triggered by toggling the RUN pin or cycling the digital core supply (DVDD)
The bootrom checks the watchdog scratch registers for a magic number on boot. You can use this to soft reset the chip
into user-specified code. See Section 5.2.4 for more information.
NOTE
Additional general-purpose scratch registers are available in POWMAN SCRATCH0 through SCRATCH7. These
registers also survive power cycling the switched core domain.
12.9.6. Programmer’s model
The SDK provides a hardware_watchdog driver to control the watchdog.
12.9.6.1. Enabling the watchdog
SDK: https://github.com/raspberrypi/pico-sdk/blob/master/src/rp2_common/hardware_watchdog/watchdog.c Lines 47 - 76
47 // Helper function used by both watchdog_enable and watchdog_reboot
48 void _watchdog_enable(uint32_t delay_ms, bool pause_on_debug) {
49     valid_params_if(HARDWARE_WATCHDOG, delay_ms <= WATCHDOG_LOAD_BITS / (1000 *
   WATCHDOG_XFACTOR));
50     hw_clear_bits(&watchdog_hw->ctrl, WATCHDOG_CTRL_ENABLE_BITS);
51 
52     // Reset everything apart from ROSC and XOSC
53     hw_set_bits(&psm_hw->wdsel, PSM_WDSEL_BITS & ~(PSM_WDSEL_ROSC_BITS |
   PSM_WDSEL_XOSC_BITS));
54 
55     uint32_t dbg_bits = WATCHDOG_CTRL_PAUSE_DBG0_BITS |
56                         WATCHDOG_CTRL_PAUSE_DBG1_BITS |
57                         WATCHDOG_CTRL_PAUSE_JTAG_BITS;
58 
59     if (pause_on_debug) {
60         hw_set_bits(&watchdog_hw->ctrl, dbg_bits);
61     } else {
62         hw_clear_bits(&watchdog_hw->ctrl, dbg_bits);
63     }
64 
RP2350 Datasheet
12.9. Watchdog
1194


65     if (!delay_ms) {
66         hw_set_bits(&watchdog_hw->ctrl, WATCHDOG_CTRL_TRIGGER_BITS);
67     } else {
68         load_value = delay_ms * (1000 * WATCHDOG_XFACTOR);
69         if (load_value > WATCHDOG_LOAD_BITS)
70             load_value = WATCHDOG_LOAD_BITS;
71 
72         watchdog_update();
73 
74         hw_set_bits(&watchdog_hw->ctrl, WATCHDOG_CTRL_ENABLE_BITS);
75     }
76 }
12.9.6.2. Updating the watchdog counter
SDK: https://github.com/raspberrypi/pico-sdk/blob/master/src/rp2_common/hardware_watchdog/watchdog.c Lines 24 - 28
24 static uint32_t load_value;
25 
26 void watchdog_update(void) {
27     watchdog_hw->load = load_value;
28 }
12.9.6.3. Usage
The Pico Examples repository provides a hello_watchdog example that uses the hardware_watchdog to demonstrate use of
the watchdog.
Pico Examples: https://github.com/raspberrypi/pico-examples/blob/master/watchdog/hello_watchdog/hello_watchdog.c Lines 11 - 33
11 int main() {
12     stdio_init_all();
13 
14     if (watchdog_enable_caused_reboot()) {
15         printf("Rebooted by Watchdog!\n");
16         return 0;
17     } else {
18         printf("Clean boot\n");
19     }
20 
21     // Enable the watchdog, requiring the watchdog to be updated every 100ms or the chip will
   reboot
22     // second arg is pause on debug which means the watchdog will pause when stepping through
   code
23     watchdog_enable(100, 1);
24 
25     for (uint i = 0; i < 5; i++) {
26         printf("Updating watchdog %d\n", i);
27         watchdog_update();
28     }
29 
30     // Wait in an infinite loop and don't update the watchdog so it reboots us
31     printf("Waiting to be rebooted by watchdog\n");
32     while(1);
33 }
RP2350 Datasheet
12.9. Watchdog
1195

