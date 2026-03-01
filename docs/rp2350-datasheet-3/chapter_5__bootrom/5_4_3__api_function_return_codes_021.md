---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.4.3. API function return codes
pages: 379-379
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 5.4.3. API function return codes

![Page 379 figure](images/fig_p0379.png)

RP2350 Datasheet

uint32_t rom_table_code(char c1, char c2) {

   return (c2 << 8) | c1;

}

These codes are also available in bootrom.h in the SDK as #defines.

5.4.2. API function availability

Some functions are not available under all architectures or security levels. The API listing in Section 5.4.6 uses the

following terms to list the availability of each individual API entry point:

Arm-S

The function is available to Secure Arm code. The majority of functions are available for Arm-S unless they deal

specifically with RISC-V or Non-secure functionality.

RISC-V

The function is available to RISC-V code. Most of the functions that are available under Arm-S are also exposed

under RISC-V unless they deal specifically with Arm security states.

Arm-NS

The function is available to Non-secure Arm code. The function in this case performs additional permission and

argument checks to prevent Secure data from leaking or being corrupted.

Each individual Arm-NS API function must be explicitly enabled by Secure code before use, via set_ns_api_permission(). A

disabled Non-secure API returns BOOTROM_ERROR_NOT_PERMITTED if disabled by Secure code. All Non-secure APIs are

disabled initially. There is no permission control on Non-secure code calling Secure-only Arm-S functions, but such a call

will crash when it attempts to access Secure-only hardware.

The Arm-NS functions may escalate through a Secure Gateway (SG) instruction to allow Non-secure code to perform

limited operations on nominally Secure-only hardware, such as QSPI direct-mode interface used for flash programming.

The RISC-V functions do not have separate entry points based on privilege level. Both M-mode and U-mode software can

call bootrom APIs, assuming they have execute permissions on ROM addresses in the PMP. However, U-mode calls will

crash if they attempt to access M-mode-only hardware.

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
