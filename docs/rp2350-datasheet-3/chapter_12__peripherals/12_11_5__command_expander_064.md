---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.11.5. Command expander
pages: 1207-1208
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 12.11.5. Command expander

RP2350 Datasheet

clock outputs, connect the clock to two pins, and invert one of them.

The CSR.CLKPHASE field defines the initial phase (count) of the clock generator, configured in units of one half HSTX

clock cycle. The clock generator resets whenever CSR.EN is low and holds at this initial phase. Once CSR.EN is set and

the output shift register begins to shift, the clock generator advances.

Clock generator output whilst CSR.EN is low is determined by the relation of clock period and initial clock phase: if the

initial clock phase is less than one half clock period, then the output is initially low. Otherwise, it is initially high. The

clock generator can be thought of as being low for the first half of each generation period, and high for the second half.

The maximum CSR.CLKPHASE is only 15 half HSTX clock cycles. The maximum CSR.CLKDIV is 16 full HSTX clock

cycles: initial phases of greater than or equal to 180 degrees with the maximum clock period require the inversion of the

clock using the bit crossbar inversion controls.

Only change CSR.CLKPHASE and CSR.CLKDIV when CSR.EN is low. It is safe to modify them in the same register write

that sets EN from low to high.

12.11.4.1. Example: centre-aligned clock

When transmitting source-synchronous data, the data sink (the receiver) must not see data transitions too late before or

too soon after the active edges of the clock. Violating these setup and hold constraints can lead to undefined operation

of the external data sink.

Since the HSTX output delays are all mutually balanced, you can meet these constraints by placing clock transitions

halfway between data transitions, known as centre-aligned clocking.

Since this positions the clock with a temporal resolution of one half of a bit time, the maximum data rate is one bit per

HSTX clock cycle per pin. Because the clock already uses DDR, you cannot use DDR to increase the data rate. Therefore

for all BIT0 through BIT7, BITx.SEL_N is equal to BITx.SEL_P.

For single-data-rate data, with an active rising edge, use the following clock generator settings:

• CSR.CLKDIV = 1 (1 HSTX clock period)
• CSR.CLKPHASE = 1 (1/2 HSTX clock period)

The clock is delayed by half an HSTX cycle, to offset it from the launch of the first data.

For single-data-rate data, with an active falling edge, use the following clock generator settings:

• CSR.CLKDIV = 1 (1 HSTX clock period)
• CSR.CLKPHASE = 2 (1 HSTX clock period)

Alternatively, you could use the same settings as an active-rising edge clock, with the clock output inverted via the bit

crossbar configuration.

For double-data-rate data, with active rising and active falling edges, use the following clock generator settings:

• CSR.CLKDIV = 2 (2 HSTX clock period)
• CSR.CLKPHASE = 1 (1/2 HSTX clock period)

In all three cases, the data rate is the same, at 1 bit per HSTX clock cycle, per pin.

12.11.5. Command expander

12.11. HSTX
1206

![Page 1208 figure](images/fig_p1208.png)

RP2350 Datasheet

Figure 128. A mixture

of commands and

Command + Count 

data are popped from

Register (16 bits)

the FIFO. Data can be

repeated or shifted

through the expansion

shift register, and

From

To output 
shift register

Expansion Shift 
Register (32 bits)

Encoder

optionally passed

FIFO

through an encoder

before passing on to

the output shift

register.

Right-rotate 
x_SHIFT 0 to 31

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
