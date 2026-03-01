---
source_pdf: rp2350-datasheet-7.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.1.3. GPIO control
pages: 40-42
type: technical_spec
generated_at: 2026-03-01T06:07:53.318745+00:00
---

# 3.1.3. GPIO control

3.1.3. GPIO control

The SIO GPIO registers control GPIOs which have the SIO function selected (function 5). This function is supported on

the following pins:

• All user GPIOs (GPIOs 0 through 29, or 0 through 47, depending on package option)
• QSPI pins
• USB DP/DM pins

All SIO GPIO control registers come in pairs. The lower-addressed register in each pair (for example, GPIO_IN) is

connected to GPIOs 0 through 31, and the higher-addressed register in each pair (for example, GPIO_HI_IN) is

connected to GPIOs 32 through 47, the QSPI pins, and the USB DP/DM pins.

NOTE

To drive a pin with the SIO’s GPIO registers, the GPIO multiplexer for this pin must first be configured to select the

SIO GPIO function. See Table 646.

These GPIO registers are shared between the two cores: both cores can access them simultaneously. There are three

groups of registers:

• Output registers, GPIO_OUT and GPIO_HI_OUT set the output level of the GPIO. 0 for low output, 1 for high output.
• Output enable registers, GPIO_OE and GPIO_HI_OE, are used to enable the output driver. 0 for high-impedance, 1

for drive high or low based on GPIO_OUT and GPIO_HI_OUT.
• Input registers, GPIO_IN and GPIO_HI_IN, allow the processor to sample the current state of the GPIOs.

Reading GPIO_IN returns up to 32 input values in a single read, and software then masks out individual pins it is

interested in.

SDK: https://github.com/raspberrypi/pico-sdk/blob/master/src/rp2_common/hardware_gpio/include/hardware/gpio.h Lines 869 - 879

869 static inline bool gpio_get(uint gpio) {

870 #ifdef NUM_BANK0_GPIOS <= 32

871     return sio_hw->gpio_in & (1u << gpio);

872 #else

873     if (gpio < 32) {

874         return sio_hw->gpio_in & (1u << gpio);

875     } else {

876         return sio_hw->gpio_hi_in & (1u << (gpio - 32));

877     }

878 #endif

879 }

The OUT and OE registers also have atomic SET, CLR, and XOR aliases. This allows software to update a subset of the pins in

one operation. This ensures safety for concurrent GPIO access, both between the two cores and between a single core’s

interrupt handler and foreground code.

SDK: https://github.com/raspberrypi/pico-sdk/blob/master/src/rp2_common/hardware_gpio/include/hardware/gpio.h Lines 918 - 924

918 static inline void gpio_set_mask(uint32_t mask) {

919 #ifdef PICO_USE_GPIO_COPROCESSOR

920     gpioc_lo_out_set(mask);

3.1. SIO
39

RP2350 Datasheet

921 #else

922     sio_hw->gpio_set = mask;

923 #endif

924 }

SDK: https://github.com/raspberrypi/pico-sdk/blob/master/src/rp2_common/hardware_gpio/include/hardware/gpio.h Lines 965 - 971

965 static inline void gpio_clr_mask(uint32_t mask) {

966 #ifdef PICO_USE_GPIO_COPROCESSOR

967     gpioc_lo_out_clr(mask);

968 #else

969     sio_hw->gpio_clr = mask;

970 #endif

971 }

SDK: https://github.com/raspberrypi/pico-sdk/blob/master/src/rp2_common/hardware_gpio/include/hardware/gpio.h Lines 1155 - 1180

1155 static inline void gpio_put(uint gpio, bool value) {

1156 #ifdef PICO_USE_GPIO_COPROCESSOR

1157     gpioc_bit_out_put(gpio, value);

1158 #elif NUM_BANK0_GPIOS <= 32

1159     uint32_t mask = 1ul << gpio;

1160     if (value)

1161         gpio_set_mask(mask);

1162     else

1163         gpio_clr_mask(mask);

1164 #else

1165     uint32_t mask = 1ul << (gpio & 0x1fu);

1166     if (gpio < 32) {

1167         if (value) {

1168             sio_hw->gpio_set = mask;

1169         } else {

1170             sio_hw->gpio_clr = mask;

1171         }

1172     } else {

1173         if (value) {

1174             sio_hw->gpio_hi_set = mask;

1175         } else {

1176             sio_hw->gpio_hi_clr = mask;

1177         }

1178     }

1179 #endif

1180 }

If both processors write to an OUT or OE register (or any of its SET/CLR/XOR aliases) on the same clock cycle, the result is as

though core 0 wrote first, then core 1 wrote immediately afterward. For example, if core 0 SETs a bit and core 1 XORs it

on the same clock cycle, the bit ends up with a value of 0.

3.1. SIO
40

RP2350 Datasheet

NOTE

This is a conceptual model for the result produced when two cores write to a GPIO register simultaneously. The

register never contains the intermediate values at any point. In the previous example, if the pin is initially 0, and core

0 performs a SET while core 1 performs a XOR, the GPIO output remains low throughout the clock cycle.

As well as being shared between cores, the GPIO registers are also shared between security domains. The Secure and

Non-secure SIO offer alternative views of the same GPIO registers, which are always mapped as GPIO function 5.

However, the Non-secure SIO can only access pins which are enabled in the GPIO Non-secure mask configured by the

ACCESSCTRL registers GPIO_NSMASK0 and GPIO_NSMASK1. The layout of the NSMASK registers matches the layout

of the SIO registers — for example, QSPI_SCK is bit 26 in both GPIO_HI_IN and GPIO_NSMASK1.

When a pin is not enabled in Non-secure code:

• Writes to the corresponding GPIO registers from a Non-secure context have no effect.
• Reads from a Non-secure context return zeroes.
• Reads and writes from a Secure context function as usual using the Secure bank.

The GPIO coprocessor port (Section 3.6.1) provides dedicated instructions for accessing the SIO GPIO registers from

the Cortex-M33 processors. This includes the ability to read and write 64 bits in a single operation.
