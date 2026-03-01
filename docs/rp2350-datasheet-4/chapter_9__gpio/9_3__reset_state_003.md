---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 9. GPIO
section: 9.3. Reset state
pages: 589-589
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 9.3. Reset state

![Page 589 figure](images/fig_p0589.png)

9.3. Reset state

At first power up, Bank 0 IOs (GPIOs 0 through 29 in the QFN-60 package, and GPIOs 0 through 47 in the QFN-80

package) assume the following state:

• Output buffer is high-impedance
• Input buffer is disabled
• Pulled low
• Isolation latches are set to latched (Section 9.7)

The pad output disable bit (GPIO0.OD) for each pad is clear at reset, but the IO muxing is reset to the null function,

9.2. Changes from RP2040
588
