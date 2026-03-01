---
source_pdf: rp2350-datasheet-10.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.11.5. Command expander
pages: 1207-1209
type: technical_spec
generated_at: 2026-03-01T12:20:14.494106+00:00
---

# 12.11.5. Command expander

12.11.5. Command expander

12.11. HSTX
1206

RP2350 Datasheet

![Page 1208 figure](images/fig_p1208.png)

*Figure 128. A mixture of commands and data are popped from the FIFO. Data can be repeated or shifted through the expansion shift register, and optionally passed through an encoder before passing on to the output shift register.*

The command expander can be inserted inline between the data FIFO and the output shift register to manipulate the

stream of data words. In general, the output stream is larger than the input stream, hence the name expander. The

commander expander is enabled by setting CSR.EXPAND_EN. Only modify this field when CSR.EN is low. It is safe to

modify this field in the same register write that sets EN from low to high. When the command expander is disabled, data

passes directly from the data FIFO to the output shift register without being modified by the expander.

When the command expander is enabled, the data FIFO carries a mixture of data and commands for the expander. Each

command consists of a 4-bit opcode and a 12-bit length, packed in the 16 LSBs of a data FIFO word, with the opcode in

bits 15 through 12, and the length in bits 11 through 0. The available commands are:

• 0x0: RAW
• 0x1: RAW_REPEAT
• 0x2: TMDS
• 0x3: TMDS_REPEAT
• 0xf: NOP

When the HSTX is first enabled, if the command expander is enabled, it expects the first word in the data FIFO to be a

command. If this command is not a NOP, it will be followed by some amount of data, then another command. Operation

continues in this manner, with runs of data interspersed with commands. A command always acts as a prefix to the

data that follows it in the FIFO.

The count field determines the number of words output by this command to the output shift register downstream, from

1 to 4095. A count of 0 is reserved to mean "infinite". The number of words that this command reads from the data FIFO

in order to produce the specified quantity of downstream data depends on the command and the

EXPAND_SHIFT.ENC_N_SHIFTS and EXPAND_SHIFT.RAW_N_SHIFTS register fields.

The expansion shift register always pops from the FIFO once at the beginning of the command. After this point,

commands bearing the x_REPEAT suffix continue to circulate the same contents through the shift register, rotating right

by EXPAND_SHIFT.ENC_SHIFT or EXPAND_SHIFT.RAW_SHIFT each time the output shift register pulls new data from

the command expander. Use a shift of 0 to repeat identical data without shifting. This is useful, for example, for

transmitting runs of the same TMDS control symbol during horizontal blanking periods in DVI.

RP2350 only implements a TMDS encoder, reserving the remaining opcode space for additional encoders in the future.

RAW and RAW_REPEAT commands bypass the encoder. TMDS and TMDS_REPEAT commands are TMDS-encoded before being

passed to the output shift register. NOP commands have no data, therefore whether they bypass the encoder or not is a

philosophical question beyond the scope of this datasheet.

The EXPAND_SHIFT register has two copies for each of its fields. Fields prefixed with RAW_ are used for RAW and

RAW_REPEAT commands. All other commands use fields prefixed with ENC_, which pass through the encoder. For example,

in DVI, TMDS control symbols using RAW_REPEAT commands may be unshifted. Pixel data using TMDS commands may be

shifted out one pixel at a time, so it is useful to have banked shift controls.

The EXPAND_SHIFT.ENC_N_SHIFTS and EXPAND_SHIFT.RAW_N_SHIFTS fields control how often the expansion shift

register is refilled for encoded and raw commands respectively. x_REPEAT commands ignore these fields since they never

refill from the FIFO, and function similarly to the CSR.N_SHIFTS field that controls the output shift register.

The command expander can only pop from the data FIFO once per cycle, so heavy use of commands (particularly NOP

12.11. HSTX
1207

RP2350 Datasheet

commands) can impact HSTX throughput. For use cases that output from the HSTX on every cycle, configure the output

shift register with CSR.N_SHIFTS > 1. This is required because the command expander cannot output data on the cycle

where it pops a command from the FIFO, so the expansion shift register is empty for at least one cycle.
