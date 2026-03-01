---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 11. PIO
section: 11.6.10. Further examples
pages: 940-940
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 11.6.10. Further examples

![Page 940 figure](images/fig_p0940.png)

Raspberry Pi Pico-series C/C++ SDK has a PIO chapter which goes into depth on some software-centric topics not

presented here. It includes a PIO + DMA logic analyser example that can sample every GPIO on every cycle (a bandwidth

of nearly 4Gbps at 125 MHz, although this does fill up RP2350’s RAM somewhat quickly).

There are also further examples in the pio/ directory in the Pico Examples repository.

Some of the more experimental example code, such as DPI and SD card support, is currently located in the Pico Extras

and Pico Playground repositories. The PIO parts of these are functional, but the surrounding software stacks are still in
