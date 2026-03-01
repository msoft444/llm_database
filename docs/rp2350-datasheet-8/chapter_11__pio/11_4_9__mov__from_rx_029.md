---
source_pdf: rp2350-datasheet-8.pdf
repository: llm_database
chapter: Chapter 11. PIO
section: 11.4.9. MOV (from RX)
pages: 898-899
type: technical_spec
generated_at: 2026-03-01T08:45:43.944040+00:00
---

# 11.4.9. MOV (from RX)

11.4.9. MOV (from RX)

11.4.9.1. Encoding

| Bit | 15 | 14 | 13 | 12 | 11 | 10 | 9 | 8 | 7 | 6 | 5 | 4 | 3 | 2 | 1 | 0 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| MOV | 1 | 0 | 0 |  | Del | ay/side | -set |  | 1 | 0 | 0 | 1 | IdxI |  | Index |  |

11.4. Instruction Set
897

RP2350 Datasheet

11.4.9.2. Operation

Read the selected RX FIFO entry into the OSR. The PIO state machine can read the FIFO entries in any order, indexed

either by the Y register, or an immediate Index in the instruction. Requires the SHIFTCTRL_FJOIN_RX_GET configuration field

to be set, otherwise its operation is undefined.

If IdxI (index by immediate) is set, the RX FIFO’s registers are indexed by the two least-significant bits of the Index

operand. Otherwise, they are indexed by the two least-significant bits of the Y register. When IdxI is clear, all non-zero

values of Index are reserved encodings, and their operation is undefined.

When only SHIFTCTRL_FJOIN_RX_GET is set, the system can also write the RX FIFO registers with random access via

RXF0_PUTGET0 through RXF0_PUTGET3 (where RXFx indicates which state machine’s FIFO is being accessed). In this

state, the RX FIFO register storage is repurposed as additional configuration registers, which the system can update at

any time and the state machine can read at any time. For example, a UART TX program might use these registers to

configure the number of data bits, or the presence of an additional stop bit.

When both SHIFTCTRL_FJOIN_RX_PUT and SHIFTCTRL_FJOIN_RX_GET are set, the system can no longer access the RX FIFO

storage registers, but the state machine can now put/get the registers in arbitrary order, allowing them to be used as

additional scratch storage.

NOTE

The RX FIFO storage registers have only a single read port and write port, and access through each port is assigned

to only one of (system, state machine) at any time.

11.4.9.3. Assembler syntax

```c
mov osr, rxfifo[y]
mov osr, rxfifo[<index>]
```

where:

| y | The literal token "y", indicating the RX FIFO entry is indexed by the Y register. |
| --- | --- |
| <index> | A value (see Section 11.3.2) specifying the RX FIFO entry to read (valid range 0-3). |
