---
source_pdf: rp2350-datasheet-8.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.1.6. Block versioning
pages: 358-358
type: technical_spec
generated_at: 2026-03-01T08:45:43.944040+00:00
---

# 5.1.6. Block versioning

5.1.6. Block versioning

Any block may contain a version. Version information consists of a tuple of either two or three 16-bit values:

(rollback).major.minor, where the rollback part is optional. An item of type VERSION contains the binary data structure

which defines the version of a block.

The rollback version may only be specified for IMAGE_DEFs and defaults to zero if not present. You cannot specify this

version for partition tables. The rollback version can be used on a secured RP2350, where it, along with a current

rollback verson number stored in OTP, can prevent installation of older, vulnerable code once a newer version is

installed (Section 5.1.11).

The full version number can be used to pick the latest version between two IMAGE_DEFs or two PARTITION_TABLEs (see

Section 5.1.7). Versions compare in lexicographic order:

1. If version x has a different rollback version than version y, then the greater rollback version determines which

version is greater overall

2. Else if version x has a different major version than version y, then the greater major version determines which

version is greater overall

3. Else the minor version determines which of x and y is greater

See Section 5.9.2.1 for full details on the VERSION item in a block.
