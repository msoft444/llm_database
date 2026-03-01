---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.8.4. Recovering from a stuck interface
pages: 418-418
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 5.8.4. Recovering from a stuck interface

![Page 418 figure](images/fig_p0418.png)

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

5.8. UART boot
417
