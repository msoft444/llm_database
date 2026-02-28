---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.8.4. Recovering from a stuck interface
pages: 418-418
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 5.8.4. Recovering from a stuck interface

Command
(ASCII)
Command
(hex)
Description
c
0x63
Clear the read/write pointer. The pointer resets to the first location in SRAM 0x20000000, and
you can begin a new read or write sequence from there. Echoes the command byte, 'c'.
x
0x78
Execute the payload that has been written to memory. Echoes the command byte, 'x', and
then reboots, passing a RAM boot search window spanning all of main SRAM. If a valid binary
was successfully written into SRAM before sending this command, it will execute.
Unrecognised commands are echoed with no other effect. More commands may be added in future versions.
5.8.3. UART boot programming flow
1. Reset or power down the RP2350 device.
2. Drive CSn low to select BOOTSEL, and SD1 high to select UART.
3. Release the reset or power up the device.
4. Wait for the splash string to be transmitted on QSPI SD2 (TX).
5. Transmit the knock sequence 0x56, 0xff, 0x8b, 0xe4 on QSPI SD3 (RX)
6. Send a 'n' nop command to ensure the interface is awake; if there is no reply, send the knock sequence again.
7. Send 'w' commands until your entire write payload transfers.
8. (Optional) Send a 'c' clear command to reset the address pointer, and then send 'r' read commands to read back
and verify the payload.
9. Send an 'x' execute command to attempt to run the payload.
There is no feedback from UART boot after echoing the final 'x' command. At this point the device reboots to attempt a
RAM image boot on the data loaded by the Non-secure UART bootloader. If the RAM image boot fails, the bootrom falls
through to the next boot source, continuing the normal boot flow. Maintaining CSn driven low and SD1 driven high will
cause the bootrom to fall through back to UART boot a second time, re-sending the UART splash screen: this indicates
the bootrom failed to recognise the UART boot binary.
5.8.4. Recovering from a stuck interface
Noise on the GPIOs may cause the UART boot shell to stop replying to commands, for example because it thinks the
host is part way through a write payload, and the host thinks that it is not. To resynchronise to the start of the next
command:
1. Wait 1 ms for the link to quiesce
2. Send 33 'n' NOP commands (size of longest command)
3. Wait 1 ms and flush your receive data
4. Send 1 'n' NOP command and confirm the device responds with an echoed NOP
If the interface fails to recover, reboot the device and try again. Failure may be caused by:
• Noise on GPIOs (particularly over long traces or wires)
• Incorrect baud rate matching
• An unstable frequency reference on XOSC XIN
• Mismatch of voltage levels (for example a QSPI_IOVDD of 1.8 V on RP2350, and a 3.3 V IO voltage on the host)
RP2350 Datasheet
5.8. UART boot
417

