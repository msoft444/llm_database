---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 11. PIO
section: 11.3.1. Directives
pages: 886-887
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 11.3.1. Directives

RP2350 Datasheet

• Asserting system level interrupts from a state machine program, and optionally waiting for the interrupt to be

acknowledged
• Synchronising execution between two state machines

State machines interact with the flags via the IRQ and WAIT instructions.

11.2.8. Interactions between state machines

Instruction memory is implemented as a 1-write, 4-read register file, allowing all four state machines to read an

instruction on the same cycle without stalling.

There are three ways to apply the multiple state machines:

• Pointing multiple state machines at the same program
• Pointing multiple state machines at different programs
• Using multiple state machines to run different parts of the same interface, e.g. TX and RX side of a UART, or

clock/hsync and pixel data on a DPI display

State machines cannot communicate data, but they can synchronise with one another by using the IRQ flags. There are

8 flags total. Each state machine can set or clear any flag using the IRQ instruction, and can wait for a flag to go high or

low using the WAIT IRQ instruction. This allows cycle-accurate synchronisation between state machines.

11.3. PIO assembler (pioasm)

The PIO Assembler parses a PIO source file and outputs the assembled version ready for inclusion in an RP2350

application. This includes C and C++ applications built against the SDK, and Python programs running on the RP2350

MicroPython port.

This section briefly introduces the directives and instructions that can be used in pioasm input. For a deeper discussion

of how to use pioasm, how it is integrated into the SDK build system, extended features such as code pass through, and

the various output formats it can produce, see Raspberry Pi Pico-series C/C++ SDK.

11.3.1. Directives

The following directives control the assembly of PIO programs:

.define (PUBLIC) <symbol> <value>

Define an integer symbol named <symbol> with the value <value> (see Section 11.3.2). If this .define appears before

the first program in the input file, then this define is global to all programs, otherwise it is local to the program in

which it occurs. If PUBLIC is specified, the symbol will be emitted into the assembled output for use by user code. For

the SDK this takes the following forms:

• #define <program_name> <symbol> value: for program symbols
• #define <symbol> value: for global symbols

.clock_div <divider>

If this directive is present, <divider> is the state machine clock divider for the program. Note, that divider is a floating

point value, but may not currently use arithmetic expressions or defined values. This directive affects the default

state machine configuration for a program. This directive is only valid within a program before the first instruction.

.fifo <fifo_config>

If this directive is present, it is used to specify the FIFO configuration for the program. It affects the default state

machine configuration for a program, but also restricts what instructions may be used (for example PUSH makes

11.3. PIO assembler (pioasm)
885

RP2350 Datasheet

no sense if there is no IN FIFO configured).

This directive supports the following configuration values:

• txrx: 4 FIFO entries for each of TX and RX; this is the default.
• tx: All 8 FIFO entries for TX.
• rx: All 8 FIFO entries for RX.
• txput: 4 FIFO entries for TX, and 4 FIFO entries for mov rxfifo[index], isr aka put. This value is not supported on

PIO version 0.
• txget: 4 FIFO entries for TX, and 4 FIFO entries for mov osr, rxfifo[index] aka get. This value is not supported on

PIO version 0.
• putget: 4 FIFO entries for mov rxfifo[index], isr aka put, and 4 FIFO entries for mov osr, rxfifo[index] aka get.

This value is not supported on PIO version 0.

This directive is only valid within a program before the first instruction.

.mov_status rxfifo < <n>

.mov_status txfifo < <n>

.mov_status irq <(prev|next)> set <n>

This directive configures the source for the mov , STATUS. One of the three syntaxes can be used to set the status

based on the RXFIFO level being below a value N, the TXFIFO level being below a value N, or an IRQ flag N being set

on this PIO instance (or the next lower numbered, or higher numbered PIO instance if prev or next or specified).

Note, that the IRQ option requires PIO version 1.

This directive affects the default state machine configuration for a program. This directive is only valid within a

program before the first instruction.

.in <count> (left|right) (auto) (<threshold>)

If this directive is present, <count> indicates the number of IN bits to be used. 'left' or 'right' if specified, control the

ISR shift direction; 'auto', if present, enables "auto-push"; <threshold>, if present, specifies the "auto-push" threshold.

This directive affects the default state machine configuration for a program.

This directive is only valid within a program before the first instruction. When assembling for PIO version 0, <count>

must be 32.

.program <name>

Start a new program with the name <name>. Note that that name is used in code so should be

alphanumeric/underscore not starting with a digit. The program lasts until another .program directive or the end of

the source file. PIO instructions are only allowed within a program.

.origin <offset>

Optional directive to specify the PIO instruction memory offset at which the program must load. Most commonly

this is used for programs that must load at offset 0, because they use data based JMPs with the (absolute) jmp

target being stored in only a few bits. This directive is invalid outside a program.

.out <count> (left|right) (auto) (<threshold>)

If this directive is present, <count> indicates the number of OUT bits to be used. 'left' or 'right' if specified control the

OSR shift direction; 'auto', if present, enables "auto-pull"; <threshold>, if present, specifies the "auto-pull" threshold.

This directive affects the default state machine configuration for a program. This directive is only valid within a

program before the first instruction.

.pio_version <version>

This directive sets the target PIO hardware version. The version for RP2350 is 1 or RP2350, and is also the default

version number. For backwards compatibility with RP2040, 0 or RP2040 may be used.

If this directive appears before the first program in the input file, then this define is the default for all programs,

otherwise it specifies the version for the program in which it occurs. If specified for a program, it must occur before

the first instruction.

11.3. PIO assembler (pioasm)
886
