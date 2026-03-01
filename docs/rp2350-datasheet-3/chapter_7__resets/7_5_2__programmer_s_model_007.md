---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 7. Resets
section: 7.5.2. Programmer’s model
pages: 504-505
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 7.5.2. Programmer’s model

![Page 504 figure](images/fig_p0504.png)

RP2350 Datasheet

7.5. Subsystem resets

7.5.1. Overview

The reset controller allows software to reset non-critical components in RP2350. The reset controller can reset the

following components:

• USB Controller
• PIO
• Peripherals, including UART, I2C, SPI, PWM, Timer, ADC
• PLLs
• IO and Pad registers

For a full list of components that can be reset using the reset controller, see the register descriptions (Section 7.5.3,

“List of Registers”).

When reset, components are held in reset at power-up. To use the component, software must deassert the reset.

NOTE

The SDK automatically deasserts some components after a reset.

7.5.2. Programmer’s model

The SDK uses the following struct to represent the resets registers:

SDK: https://github.com/raspberrypi/pico-sdk/blob/master/src/rp2350/hardware_structs/include/hardware/structs/resets.h Lines 63 - 159

 63 typedef struct {

 64     _REG_(RESETS_RESET_OFFSET) // RESETS_RESET

 65     // 0x10000000 [28]    USBCTRL      (1)

 66     // 0x08000000 [27]    UART1        (1)

 67     // 0x04000000 [26]    UART0        (1)

 68     // 0x02000000 [25]    TRNG         (1)

 69     // 0x01000000 [24]    TIMER1       (1)

 70     // 0x00800000 [23]    TIMER0       (1)

 71     // 0x00400000 [22]    TBMAN        (1)

 72     // 0x00200000 [21]    SYSINFO      (1)

 73     // 0x00100000 [20]    SYSCFG       (1)

 74     // 0x00080000 [19]    SPI1         (1)

 75     // 0x00040000 [18]    SPI0         (1)

 76     // 0x00020000 [17]    SHA256       (1)

 77     // 0x00010000 [16]    PWM          (1)

 78     // 0x00008000 [15]    PLL_USB      (1)

 79     // 0x00004000 [14]    PLL_SYS      (1)

 80     // 0x00002000 [13]    PIO2         (1)

 81     // 0x00001000 [12]    PIO1         (1)

 82     // 0x00000800 [11]    PIO0         (1)

 83     // 0x00000400 [10]    PADS_QSPI    (1)

 84     // 0x00000200 [9]     PADS_BANK0   (1)

 85     // 0x00000100 [8]     JTAG         (1)

 86     // 0x00000080 [7]     IO_QSPI      (1)

 87     // 0x00000040 [6]     IO_BANK0     (1)

 88     // 0x00000020 [5]     I2C1         (1)

 89     // 0x00000010 [4]     I2C0         (1)

 90     // 0x00000008 [3]     HSTX         (1)

7.5. Subsystem resets
503

RP2350 Datasheet

 91     // 0x00000004 [2]     DMA          (1)

 92     // 0x00000002 [1]     BUSCTRL      (1)

 93     // 0x00000001 [0]     ADC          (1)

 94     io_rw_32 reset;

 95 

 96     _REG_(RESETS_WDSEL_OFFSET) // RESETS_WDSEL

 97     // 0x10000000 [28]    USBCTRL      (0)

 98     // 0x08000000 [27]    UART1        (0)

 99     // 0x04000000 [26]    UART0        (0)

100     // 0x02000000 [25]    TRNG         (0)

101     // 0x01000000 [24]    TIMER1       (0)

102     // 0x00800000 [23]    TIMER0       (0)

103     // 0x00400000 [22]    TBMAN        (0)

104     // 0x00200000 [21]    SYSINFO      (0)

105     // 0x00100000 [20]    SYSCFG       (0)

106     // 0x00080000 [19]    SPI1         (0)

107     // 0x00040000 [18]    SPI0         (0)

108     // 0x00020000 [17]    SHA256       (0)

109     // 0x00010000 [16]    PWM          (0)

110     // 0x00008000 [15]    PLL_USB      (0)

111     // 0x00004000 [14]    PLL_SYS      (0)

112     // 0x00002000 [13]    PIO2         (0)

113     // 0x00001000 [12]    PIO1         (0)

114     // 0x00000800 [11]    PIO0         (0)

115     // 0x00000400 [10]    PADS_QSPI    (0)

116     // 0x00000200 [9]     PADS_BANK0   (0)

117     // 0x00000100 [8]     JTAG         (0)

118     // 0x00000080 [7]     IO_QSPI      (0)

119     // 0x00000040 [6]     IO_BANK0     (0)

120     // 0x00000020 [5]     I2C1         (0)

121     // 0x00000010 [4]     I2C0         (0)

122     // 0x00000008 [3]     HSTX         (0)

123     // 0x00000004 [2]     DMA          (0)

124     // 0x00000002 [1]     BUSCTRL      (0)

125     // 0x00000001 [0]     ADC          (0)

126     io_rw_32 wdsel;

127 

128     _REG_(RESETS_RESET_DONE_OFFSET) // RESETS_RESET_DONE

129     // 0x10000000 [28]    USBCTRL      (0)

130     // 0x08000000 [27]    UART1        (0)

131     // 0x04000000 [26]    UART0        (0)

132     // 0x02000000 [25]    TRNG         (0)

133     // 0x01000000 [24]    TIMER1       (0)

134     // 0x00800000 [23]    TIMER0       (0)

135     // 0x00400000 [22]    TBMAN        (0)

136     // 0x00200000 [21]    SYSINFO      (0)

137     // 0x00100000 [20]    SYSCFG       (0)

138     // 0x00080000 [19]    SPI1         (0)

139     // 0x00040000 [18]    SPI0         (0)

140     // 0x00020000 [17]    SHA256       (0)

141     // 0x00010000 [16]    PWM          (0)

142     // 0x00008000 [15]    PLL_USB      (0)

143     // 0x00004000 [14]    PLL_SYS      (0)

144     // 0x00002000 [13]    PIO2         (0)

145     // 0x00001000 [12]    PIO1         (0)

146     // 0x00000800 [11]    PIO0         (0)

147     // 0x00000400 [10]    PADS_QSPI    (0)

148     // 0x00000200 [9]     PADS_BANK0   (0)

149     // 0x00000100 [8]     JTAG         (0)

150     // 0x00000080 [7]     IO_QSPI      (0)

151     // 0x00000040 [6]     IO_BANK0     (0)

152     // 0x00000020 [5]     I2C1         (0)

153     // 0x00000010 [4]     I2C0         (0)

154     // 0x00000008 [3]     HSTX         (0)

7.5. Subsystem resets
504
