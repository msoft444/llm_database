---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.1.8. Hashing and signing
pages: 358-358
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 5.1.8. Hashing and signing

5.1.8. Hashing and signing

Any block may be hashed or signed. A hashed block stores the image hash value (see Section 5.9.2.3). At runtime, the

bootrom calculates a hash and compares it to the stored hash to determine if the block is valid. Hashes guard against

5.1. Bootrom concepts
357
