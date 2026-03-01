---
source_pdf: rp2350-datasheet-8.pdf
repository: llm_database
chapter: Chapter 14. Electrical and mechanical
section: 14.6. Solder profile
pages: 1332-1333
type: technical_spec
generated_at: 2026-03-01T08:45:43.944040+00:00
---

# 14.6. Solder profile

14.6. Solder profile

RP2350 is a Pb-free part, with a Tp value of 260°C.

All temperatures refer to the centre of the package, measured on the package body surface that faces up during

14.4. Package markings
1331

RP2350 Datasheet

assembly reflow (live-bug orientation). If parts are reflowed in a different orientation (e.g. dead-bug), Tp shall be within

±2°C of the live-bug Tp and still meet the Tc requirements; otherwise, you must adjust the profile to achieve the latter.

![Page 1333 figure](images/fig_p1333.png)

Figure 147.

Classification profile

(not to scale)

Reflow profiles in this document are for classification/preconditioning, and are not meant to specify board assembly

profiles. Actual board assembly profiles should be developed based on specific process needs and board designs, and

should not exceed the parameters in Table 1425.

| Profile feature | Value |
| --- | --- |
| Temperature min (T ) smin | 150°C |
| Temperature max (T ) smax | 200°C |
| Time (t) from (T to T ) s smin smax | 60 - 120 seconds |
| Ramp-up rate (T to T) L p | 3°C/second max. |
| Liquidous temperature (T) L | 217°C |
| Time (t) maintained above T L L | 60 to 150 seconds |
| Peak package body temperature (T) p | 260°C |
| Classification temperature (T) c | 260°C |
| Time (t) within 5°C of the specified classification temperature (T) p c | 30 seconds |
| Ramp-down rate (T to T) p L | 6°C/second max. |
| Time 25°C to peak temperature | 8 minutes max. |

Table 1425. Solder

14.6. Solder profile
1332
