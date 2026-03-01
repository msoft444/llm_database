---
source_pdf: rp2350-datasheet-7.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.4.3. API function return codes
pages: 379-380
type: technical_spec
generated_at: 2026-03-01T06:07:53.318745+00:00
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
| BOOTROM ERROR INVALID ADDRESS _ _ _ | -10 | An address argument was out-of-bounds or was determined to be an address that the caller may not access. |
| BOOTROM ERROR BAD ALIGNMENT _ _ _ | -11 | An address passed to the function was not correctly aligned. |
| BOOTROM ERROR INVALID STATE _ _ _ | -12 | Something happened or failed to happen in the past, and consequently the request cannot currently be serviced. |
| BOOTROM ERROR BUFFER TOO SMALL _ _ _ _ | -13 | A user-allocated buffer was too small to hold the result or working state of the function. |
| BOOTROM ERROR PRECONDITION NOT MET _ _ _ _ | -14 | The call failed because another bootrom function must be called first. |
| BOOTROM ERROR MODIFIED DATA _ _ _ | -15 | Cached data was determined to be inconsistent with the full version of the data it was copied from. |
| BOOTROM ERROR INVALID DATA _ _ _ | -16 | The contents of a data structure are invalid |
| BOOTROM ERROR NOT FOUND _ _ _ | -17 | An attempt was made to access something that does not exist; or, a search failed. |
| BOOTROM ERROR UNSUPPORTED MODIFICATION _ _ _ | -18 | Modification is impossible based on current state. This might occur, for example, when attempting to clear an OTP bit. |
| BOOTROM ERROR LOCK REQUIRED _ _ _ | -19 | A required lock is not owned. See Section 5.4.4. |

5.4. Bootrom APIs
378

RP2350 Datasheet
