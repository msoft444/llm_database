---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.5.8. Rescue reset
pages: 91-91
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 3.5.8. Rescue reset

Bits
Description
Type
Reset
0
TRACE_CAPTURE_FIFO_FLUSH: Set to 1 to continuously hold the trace FIFO in
a flushed state and prevent overflow.
Before clearing this flag, configure and start a DMA channel with the correct
DREQ for the TRACE_CAPTURE_FIFO register.
Clear this flag to begin sampling trace data, and set once again once the trace
capture buffer is full. You must configure the TPIU in order to generate trace
packets to be captured, as well as components like the ETM further upstream
to generate the event stream propagated to the TPIU.
RW
0x1
CORESIGHT_TRACE: TRACE_CAPTURE_FIFO Register
Offset: 0x4
Description
FIFO for trace data captured from the TPIU
Table 98.
TRACE_CAPTURE_FIF
O Register
Bits
Description
Type
Reset
31:0
RDATA: Read from an 8 x 32-bit FIFO containing trace data captured from the
TPIU.
Hardware pushes to the FIFO on rising edges of clk_sys, when either of the
following is true:
* TPIU TRACECTL output is low (normal trace data)
* TPIU TRACETCL output is high, and TPIU TRACEDATA0 and TRACEDATA1
are both low (trigger packet)
These conditions are in accordance with Arm Coresight Architecture Spec
v3.0 section D3.3.3: Decoding requirements for Trace Capture Devices
The data captured into the FIFO is the full 32-bit TRACEDATA bus output by
the TPIU. Note that the TPIU is a DDR output at half of clk_sys, therefore this
interface can capture the full 32-bit TPIU DDR output bandwidth as it samples
once per active edge of the TPIU output clock.
RF
0x00000000
3.5.8. Rescue reset
A rescue reset is a full system reset, similar to asserting the RUN pin low, which also sets a flag telling the bootrom to
halt before running any user software. This is performed over the SWD bus using the RP-AP, and can be performed even
when system clocks are stopped and the switched core power domain is powered down. This is used in the case where
the chip has locked up, for example if code has been programmed into flash which permanently halts the system clock:
since the debugger can no longer communicate with the processors to return the system to a working state, more
drastic action is needed. This functionality was provided by the Rescue DP on RP2040, but on RP2350 it is provided by
the RP-AP, to avoid mandatory use of multidrop SWD.
A rescue is invoked by setting and then clearing the CTRL.RESCUE_RESTART bit in the RP-AP. This causes a hard reset
of the chip, and sets CHIP_RESET.RESCUE_FLAG to indicate that a rescue reset took place. The bootrom checks this
flag almost immediately in the initial boot process (before watchdog, flash or USB boot), acknowledges by clearing the
bit, then halts the processor. This leaves the system in a safe state, with the system clock running, so that the debugger
can reattach to the cores and load fresh code.
RP2350 Datasheet
3.5. Debug
90

