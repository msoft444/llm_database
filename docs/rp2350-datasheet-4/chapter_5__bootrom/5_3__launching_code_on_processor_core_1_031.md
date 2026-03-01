---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.3. Launching code on Processor Core 1
pages: 376-376
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 5.3. Launching code on Processor Core 1

5.3. Launching code on Processor Core 1

As described in Section 5.2, after reset, processor core 1 sleeps at start-up, and remains asleep until woken by core 0

via the SIO FIFOs.

If you are using the SDK then you can use the multicore_launch_core1() function to launch code on processor core 1.

However this section describes the procedure to launch code on processor core 1 yourself.

The procedure to start running on processor core 1 involves both cores moving in lockstep through a state machine

coordinated by passing messages over the inter-processor FIFOs. This state machine is designed to be robust enough

to cope with a recently reset processor core 1 which may be anywhere in its boot code, up to and including going to

sleep. As result, the procedure may be performed at any point after processor core 1 has been reset (either by system

5.3. Launching code on Processor Core 1
375
