---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.4.7. Alphabetical list of API functions and ROM data
pages: 383-383
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 5.4.7. Alphabetical list of API functions and ROM data

RP2350 Datasheet

5.4.6.8. Miscellaneous

These functions are provided to all platforms and security levels, but perform additional checks when called from Non-

secure Arm code:

• reboot() (Arm-S, Arm-NS, RISC-V)
• otp_access() (Arm-S, Arm-NS, RISC-V)

5.4.6.9. Non-secure only

• secure_call() (Arm-NS)

5.4.6.10. Bit manipulation

Unlike RP2040, the bootrom doesn’t contain bit manipulation functions. Processors on RP2350 implement hardware

instructions for these operations which are far faster than the software implementations in the RP2040 bootrom.

5.4.6.11. Memcpy and Memset

Unlike RP2040, the bootrom doesn’t provide memory copy or clearing functions, as your language runtime is expected

to already provide well-performing implementations of these on Cortex-M33 or Hazard3.

The bootrom does contain private implementations of standard C memcpy() and memset(), for both Arm and RISC-V, but

these are optimised for size rather than performance. They are not exported in the ROM table.

5.4.6.12. Floating point

Unlike RP2040 the bootrom doesn’t contain functions for floating point arithmetic. On Arm there is standard processor

support for single-precision arithmetic via the Cortex-M FPU, and RP2350 provides an Arm coprocessor which

dramatically accelerates double-precision arithmetic (the DCP, Section 3.6.2). The SDK defaults to the most performant

hardware or software implementation available.

5.4.7. Alphabetical list of API functions and ROM data

• bootrom_state_reset() (Arm-S, RISC-V)
• chain_image() (Arm-S, RISC-V)
• connect_internal_flash() (Arm-S, RISC-V)
• flash_devinfo16_ptr (Arm-S, RISC-V)
• flash_enter_cmd_xip() (Arm-S, RISC-V)
• flash_exit_xip() (Arm-S, RISC-V)
• flash_flush_cache() (Arm-S, RISC-V)
• flash_op() (Arm-S, Arm-NS, RISC-V)
• flash_range_erase() (Arm-S, RISC-V)
• flash_range_program() (Arm-S, RISC-V)
• flash_reset_address_trans() (Arm-S, RISC-V)
• flash_runtime_to_storage_addr() (Arm-S, Arm-NS, RISC-V)

5.4. Bootrom APIs
382
