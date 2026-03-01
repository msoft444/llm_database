---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.12.3. Operation
pages: 1214-1214
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 12.12.3. Operation

RP2350 Datasheet

The TRNG block’s ROSC is a free-running oscillator with no direct connection to the system clocks on RP2350. As a

result, the ROSC generally runs asynchronously to the system clocks.

After a sufficient number of samples have been collected, the TRNG block completes the generation process and

presents the random number in the EHR_DATA[x] result registers.

For more information, see ARM IP - True Random Number Generator

12.12.2. Configuration

The TRNG block contains three different built-in entropy checking mechanisms: At reset, these are all enabled by default

and hence do not require explicit enabling.

You can configure the TRNG block in the following ways:

• Configure the frequency of the ROSC by selecting of one of four ROSC chain lengths, see TRNG_CONFIG.
• Configure the ROSC sampling period in terms of system clock ticks, see SAMPLE_CNT1.

Because the system clock generally runs much faster than the ROSC, the sampling period is expected to be at least a

few tens of system clock ticks.

Because the characteristics of the TRNG ROSC and system clock frequency will differ for each implementation of the

TRNG IP block, Arm details a TRNG characterisation procedure to determine the most appropriate ROSC chain length

and sampling frequency settings on each SoC design. For details about that characterisation procedure, see ARM

TrustZone True Number Generator.

Software drivers for the RP2350 TRNG block do not utilise the standard approach (see Section 12.12.4). As a result,

software does not configure the ROSC length and sample count settings provided by the Arm characterisation

procedure.

When configuring the TRNG block, consider the following principles:

• As average generation time increases, result quality increases and failed entropy checks decrease.
• A low sample count decreases average generation time, but increases the chance of NIST test-failing results and

failed entropy checks.

For acceptable results with an average generation time of about 2 milliseconds, use ROSC chain length settings of 0 or

1 and sample count settings of 20-25.

Larger sample count settings (e.g. 100) provide proportionately slower average generation times. These settings

significantly reduce, but do not eliminate NIST test failures and entropy check failures. Results occasionally take an

especially long time to generate.

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
