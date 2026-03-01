---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 11. PIO
section: 11.2. Programmer’s model
pages: 879-879
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 11.2. Programmer’s model

RP2350 Datasheet

RP2350 adds the following new instruction features:

• Adds PINCTRL_JMP_PIN as a source for the WAIT instruction, plus an offset in the range 0-3.

◦This gives WAIT pin arguments a per-SM mapping that is independent of the IN-mapped pins.
• Adds PINDIRS as a destination for MOV.

◦This allows changing the direction of all OUT-mapped pins with a single instruction: MOV PINDIRS, NULL or MOV

PINDIRS, ~NULL
• Adds SM IRQ flags as a source for MOV x, STATUS

◦This allows branching (as well as blocking) on the assertion of SM IRQ flags.
• Extends IRQ instruction encoding to allow state machines to set, clear and observe IRQ flags from different PIO

blocks.

◦There is no delay penalty for cross-PIO IRQ flags: an IRQ on one state machine is observable to all state

machines on the next cycle.
• Adds the FJOIN_RX_GET FIFO mode.

◦A new MOV encoding reads any of the four RX FIFO storage registers into OSR.

◦This instruction permits random reads of the four FIFO entries, indexed either by instruction bits or the Y

scratch register.
• Adds the FJOIN_RX_PUT FIFO mode.

◦A new MOV encoding writes the ISR into any of the four RX FIFO storage registers.

◦The registers are indexed either by instruction bits or the Y scratch register.

RP2350 adds the following security features:

• Limits Non-secure PIOs (set to via ACCESSCTRL) to observation of only Non-secure GPIOs. Attempting to read a

Secure GPIO returns a 0.
• Disables cross-PIO functionality (IRQs, CTRL_NEXTPREV operations) between Non-secure PIO blocks (those which

permit Non-secure access according to ACCESSCTRL) and Secure-only blocks (those which do not).

RP2350 includes the following general improvements:

• Increased the number of PIO blocks from two to three (8 → 12 state machines).
• Improved GPIO input/output delay and skew.
• Reduced DMA request (DREQ) latency by one cycle vs RP2040.

11.2. Programmer’s model

The four state machines execute from shared instruction memory. System software loads programs into this memory,

configures the state machines and IO mapping, and then sets the state machines running. PIO programs come from

various sources: assembled directly by the user, drawn from the PIO library, or generated programmatically by user

software.

From this point on, state machines are generally autonomous, and system software interacts through DMA, interrupts

and control registers, as with other peripherals on RP2350. For more complex interfaces, PIO provides a small but

flexible set of primitives which allow system software to be more hands-on with state machine control flow.

11.2. Programmer’s model
878
