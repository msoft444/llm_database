---
source_pdf: rp2350-datasheet-8.pdf
repository: llm_database
chapter: Chapter 8. Clocks
section: 8.1.4. Frequency counter
pages: 523-523
type: technical_spec
generated_at: 2026-03-01T08:45:43.944040+00:00
---

# 8.1.4. Frequency counter

RP2350 Datasheet

8.1.4. Frequency counter

The frequency counter measures the frequency of internal and external clocks by counting the clock edges seen over a

test interval. The interval is defined by counting cycles of clk_ref, which must be driven either from XOSC or a stable

external source of known frequency.

The user can pick between accuracy and test time using the FC0_INTERVAL register. Table 542, “Frequency Counter

Test Interval vs Accuracy” shows this trade off:

| Interval Register | Test Interval | Accuracy |
| --- | --- | --- |
| 0 | 1μs | 2048kHz |
| 1 | 2μs | 1024kHz |
| 2 | 4μs | 512kHz |
| 3 | 8μs | 256kHz |
| 4 | 16μs | 128kHz |
| 5 | 32μs | 64kHz |
| 6 | 64μs | 32kHz |
| 7 | 125μs | 16kHz |
| 8 | 250μs | 8kHz |
| 9 | 500μs | 4kHz |
| 10 | 1ms | 2kHz |
| 11 | 2ms | 1kHz |
| 12 | 4ms | 500Hz |
| 13 | 8ms | 250Hz |
| 14 | 16ms | 125Hz |
| 15 | 32ms | 62.5Hz |

Table 542. Frequency

Counter Test Interval

vs Accuracy
