---
source_pdf: rp2350-datasheet-7.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.5.4. Debug power domains
pages: 88-88
type: technical_spec
generated_at: 2026-03-01T06:07:53.318745+00:00
---

# 3.5.4. Debug power domains

3.5.4. Debug power domains

The SW-DP and the RP-AP are in the always-on power domain. This means they are available even when the system is in

its lowest-power state, with the switched core domain (which includes the processors) fully powered down.

The remainder of the debug hardware is in the switched core domain. This is the same domain as the processors and

system peripherals.

Setting the CDBGPWRUPREQ bit in the SW-DP’s CTRL/STAT register will force a power up of the switched core domain,

making the remaining debug hardware available. This power up takes some time, as it is sequenced by the 32 kHz low-

power oscillator (Section 8.4), so the CDBGPWRUPACK bit must be polled to wait for the system to power up before

attempting to access any APs other than the RP-AP. See Arm’s ADIv6 specification for the SW-DP’s register listing.

Note that the RP-AP is accessible without asserting CDBGPWRUPREQ, as it is always powered.
