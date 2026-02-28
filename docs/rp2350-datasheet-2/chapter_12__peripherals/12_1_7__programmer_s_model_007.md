---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.1.7. Programmer’s model
pages: 971-972
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 12.1.7. Programmer’s model

RP2350 Datasheet

trigger level.
• The FIFOs are disabled (have a depth of one location) and there is no data present in the transmit FIFO. This

asserts the transmit interrupt HIGH. To clear the transmit interrupt, perform a single write to the transmit FIFO.

In both cases, you can also clear the interrupt manually.

To update the transmit FIFO, write data to the transmit FIFO before or after enabling the UART and the interrupts.

NOTE

The transmit interrupt is based on a transition through a level, rather than on the level itself. When the interrupt and

the UART is enabled before any data is written to the transmit FIFO, the interrupt is not set. The interrupt is only set

after written data leaves the single location of the transmit FIFO and it becomes empty.

12.1.6.4. UARTRTINTR

The receive timeout interrupt is asserted when the receive FIFO is not empty and no more data is received during a 32-

bit period.

The receive timeout interrupt is cleared in the following scenarios:

• the FIFO becomes empty through reading all the data or by reading the holding register
• a 1 is written to the corresponding bit of the Interrupt Clear Register, UARTICR

12.1.6.5. UARTEINTR

The error interrupt is asserted when an error occurs in the reception of data by the UART. The interrupt can be caused

by a number of different error conditions:

• framing
• parity
• break
• overrun

To determine the cause of the interrupt, read the Raw Interrupt Status Register (UARTRIS) or the Masked Interrupt Status

Register (UARTMIS). To clear the interrupt, write to the relevant bits of the Interrupt Clear Register, UARTICR (bits 7 to 10 are

the error clear bits).

12.1.6.6. UARTINTR

The interrupts are also combined into a single output, that is an OR function of the individual masked sources. You can

connect this output to a system interrupt controller to provide another level of masking on a individual peripheral basis.

The combined UART interrupt is asserted if any of the individual interrupts are asserted and enabled.

12.1.7. Programmer’s model

The SDK provides a uart_init function to configure the UART with a particular baud rate. Once the UART is initialised,

the user must configure a GPIO pin as UART_TX and UART_RX. See Section 9.10.1 for more information on selecting a GPIO

function.

To initialise the UART, the uart_init function takes the following steps:

1. De-asserts the reset

12.1. UART
970

RP2350 Datasheet

2. Enables clk_peri

3. Sets enable bits in the control register

4. Enables the FIFOs

5. Sets the baud rate divisors

6. Sets the format

SDK: https://github.com/raspberrypi/pico-sdk/blob/master/src/rp2_common/hardware_uart/uart.c Lines 42 - 92

42 uint uart_init(uart_inst_t *uart, uint baudrate) {

43     invalid_params_if(HARDWARE_UART, uart != uart0 && uart != uart1);

44 

45     if (uart_clock_get_hz(uart) == 0) {

46         return 0;

47     }

48 

49     uart_reset(uart);

50     uart_unreset(uart);

51 

52     uart_set_translate_crlf(uart, PICO_UART_DEFAULT_CRLF);

53 

54     // Any LCR writes need to take place before enabling the UART

55     uint baud = uart_set_baudrate(uart, baudrate);

56 

57     // inline the uart_set_format() call, as we don't need the CR disable/re-enable

58     // protection, and also many people will never call it again, so having

59     // the generic function is not useful, and much bigger than this inlined

60     // code which is only a handful of instructions.

61     //

62     // The UART_UARTLCR_H_FEN_BITS setting is combined as well as it is the same register

63 #ifdef 0

64     uart_set_format(uart, 8, 1, UART_PARITY_NONE);

65     // Enable FIFOs (must be before setting UARTEN, as this is an LCR access)

66     hw_set_bits(&uart_get_hw(uart)->lcr_h, UART_UARTLCR_H_FEN_BITS);

67 #else

68     uint data_bits = 8;

69     uint stop_bits = 1;

70     uint parity = UART_PARITY_NONE;

71     hw_write_masked(&uart_get_hw(uart)->lcr_h,

72         ((data_bits - 5u) << UART_UARTLCR_H_WLEN_LSB) |

73             ((stop_bits - 1u) << UART_UARTLCR_H_STP2_LSB) |

74             (bool_to_bit(parity != UART_PARITY_NONE) << UART_UARTLCR_H_PEN_LSB) |

75             (bool_to_bit(parity == UART_PARITY_EVEN) << UART_UARTLCR_H_EPS_LSB) |

76             UART_UARTLCR_H_FEN_BITS,

77         UART_UARTLCR_H_WLEN_BITS | UART_UARTLCR_H_STP2_BITS |

78             UART_UARTLCR_H_PEN_BITS | UART_UARTLCR_H_EPS_BITS |

79             UART_UARTLCR_H_FEN_BITS);

80 #endif

81 

82     // Enable the UART, both TX and RX

83     uart_get_hw(uart)->cr = UART_UARTCR_UARTEN_BITS | UART_UARTCR_TXE_BITS |

   UART_UARTCR_RXE_BITS;

84     // Always enable DREQ signals -- no harm in this if DMA is not listening

85     uart_get_hw(uart)->dmacr = UART_UARTDMACR_TXDMAE_BITS | UART_UARTDMACR_RXDMAE_BITS;

86 

87     return baud;

88 }

12.1. UART
971
