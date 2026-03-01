---
source_pdf: rp2350-datasheet-8.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.12.3. Operation
pages: 1214-1214
type: technical_spec
generated_at: 2026-03-01T08:45:43.944040+00:00
---

# 12.12.3. Operation

12.12.3. Operation

To initiate TRNG generation, set the RND_SRC_EN bit in RND_SOURCE_ENABLE. The TRNG will run until:

• It has successfully completed the generation of a random number.
• One, or more, of the internal entropy checking mechanisms indicates a failed run.

In either case, you can read the resultant status from RNG_ISR.

To generate TRNG block interrupts, set bits in RNG_IMR. Use RNG_ICR to clear active interrupt status bits.

The EHR_DATA[x] registers read 0 until successful generation has occurred, so the CPU cannot read random number

results during generation,

After successful generation, read the last result register, EHR_DATA[5] to clear all of the result registers. If the result fails

an entropy check, no results are presented and the EHR_DATA[x] registers all read as 0.

After TRNG generation and when not in use, the RND_SRC_EN bit should be cleared.

12.12. TRNG
1213
