---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.4.3. API function return codes
pages: 379-379
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 5.4.3. API function return codes

5.4.3. API function return codes

Some functions do not support returning any error, and are marked void. The remainder return either 0 (BOOTROM_OK) or a

positive value (if data needs to be returned) for success. These bootrom error codes are identical to the error codes

used by the SDK, so they can be used interchangeably. This explains the gaps in the numbering for SDK error codes that

aren’t used by the bootrom.

| Name | Value | Description |
| --- | --- | --- |
| value | >= 0 | The function succeeded and returned the value |
| BOOTROM OK _ | 0 | The function executed successfully |
| BOOTROM ERROR NOT PERMITTED _ _ _ | -4 | The operation was disallowed by a security constraint |
| BOOTROM ERROR INVALID ARG _ _ _ | -5 | One or more parameters passed to the function is outside the range of supported values; BOOTROM_ERROR_INVALID_ADDRESS and BOOTROM_ERROR_BAD_ALIGNMENT are more specific errors. |

5.4. Bootrom APIs
378
