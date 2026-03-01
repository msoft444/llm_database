---
source_pdf: rp2350-datasheet-9.pdf
repository: llm_database
chapter: Chapter 8. Clocks
section: 8.3.6. Randomising the frequency
pages: 564-564
type: technical_spec
generated_at: 2026-03-01T11:44:31.548839+00:00
---

# 8.3.6. Randomising the frequency

8.3.6. Randomising the frequency

Randomisation is enabled by setting the drive strength controls for the first two stages of the ROSC loop to DS0_RANDOM

and DS1_RANDOM. An LFSR then provides the drive strength controls for those two stages which are always included in the

loop regardless of the FREQ_RANGE setting. It is recommended to randomise both stages. When the low FREQ_RANGE is

selected the randomiser will increase the frequency by up to 22% of the default. The increase will be approximately half

of that if only one stage is randomised. The LFSR can be seeded by writing to the RANDOM register. This can be done at

any time but will restart the randomiser.
