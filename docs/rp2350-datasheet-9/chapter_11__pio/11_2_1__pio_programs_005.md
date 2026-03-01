---
source_pdf: rp2350-datasheet-9.pdf
repository: llm_database
chapter: Chapter 11. PIO
section: 11.2.1. PIO programs
pages: 880-880
type: technical_spec
generated_at: 2026-03-01T11:44:31.548839+00:00
---

# 11.2.1. PIO programs

11.2.1. PIO programs

PIO state machines execute short binary programs.

Programs for common interfaces, such as UART, SPI, or I2C, are available in the PIO library. In many cases, it is not

necessary to write PIO programs. However, the PIO is much more flexible when programmed directly, supporting a wide

variety of interfaces which may not have been foreseen by its designers.

The PIO has a total of nine instructions: JMP, WAIT, IN, OUT, PUSH, PULL, MOV, IRQ, and SET. For more information about these

instructions, see Section 11.4.

Though the PIO only has a total of nine instructions, it would be difficult to edit PIO program binaries by hand. PIO

assembly is a textual format, describing a PIO program, where each command corresponds to one instruction in the

output binary. The following code snippet contains an example program written in in PIO assembly:

Pico Examples: https://github.com/raspberrypi/pico-examples/blob/master/pio/squarewave/squarewave.pio Lines 8 - 13

```c
 8 .program squarewave
 9     set pindirs, 1   ; Set pin to output
10 again:
11     set pins, 1 [1]  ; Drive pin high and then delay for one cycle
12     set pins, 0      ; Drive pin low
13     jmp again        ; Set PC to label `again`
```

The PIO assembler is included with the SDK, and is called pioasm. This program processes a PIO assembly input text file,

which may contain multiple programs, and writes out the assembled programs ready for use. For the SDK, these

assembled programs are emitted as C headers, containing constant arrays.

For more information, see Section 11.3.
