---
source_pdf: rp2350-datasheet-10.pdf
repository: llm_database
chapter: Chapter 4. Memory
section: 4.4.4. Performance counters
pages: 347-347
type: technical_spec
generated_at: 2026-03-01T12:20:14.494106+00:00
---

# 4.4.4. Performance counters

4.4.4. Performance counters

The XIP subsystem provides two performance counters. These are 32 bits in size, saturate upon reaching 0xffffffff,

and are cleared by writing any value. They count:

1. The total number of XIP accesses, to any alias

2. The number of XIP accesses that resulted in a cache hit

This provides a way to profile the cache hit rate for common use cases.
