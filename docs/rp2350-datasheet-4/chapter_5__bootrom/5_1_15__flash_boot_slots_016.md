---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.1.15. Flash boot slots
pages: 361-361
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 5.1.15. Flash boot slots

5.1.15. Flash boot slots

The previous sections within this chapter discuss block loops starting within the first 4 kB of flash. Such a block loop

contained either an IMAGE_DEF, a partition table (searched for IMAGE_DEFs), or an IMAGE_DEF and a PARTITION_TABLE (not

searched).

All the previously mentioned cases discovered their block loop in slot 0. Under certain circumstances, the neighbouring

slot 1 is also searched.

5.1. Bootrom concepts
360
