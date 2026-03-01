---
source_pdf: rp2350-datasheet-5.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.4.6. Categorised list of API functions and ROM data
pages: 381-383
type: technical_spec
generated_at: 2026-03-01T04:13:03.328193+00:00
---

# 5.4.6. Categorised list of API functions and ROM data

5.4.6. Categorised list of API functions and ROM data

The terms in parentheses after each function name (Arm-S, Arm-NS, RISC-V) indicate the architecture and security state

combinations where that API is available:

• Arm-S: Arm processors running in the Secure state
• Arm-NS: Arm processors running in the Non-secure state
• RISC-V: RISC-V processors

See Section 5.4.2 for the full definitions of these terms.

List entries ending with parentheses, such as flash_op(), are callable functions. List entries without parentheses, such

as git_revision, are pointers to ROM data locations.

5.4.6.1. Low-level Flash access

These low-level (Secure-only) flash access functions are similar to the ones on RP2040:

• connect_internal_flash() (Arm-S, RISC-V)
• flash_enter_cmd_xip() (Arm-S, RISC-V)
• flash_exit_xip() (Arm-S, RISC-V)
• flash_flush_cache() (Arm-S, RISC-V)
• flash_range_erase() (Arm-S, RISC-V)
• flash_range_program() (Arm-S, RISC-V)

These are new with RP2350:

5.4. Bootrom APIs
380

RP2350 Datasheet

• flash_reset_address_trans() (Arm-S, RISC-V)
• flash_select_xip_read_mode() (Arm-S, RISC-V)

5.4.6.2. High-level flash access

The higher level access functions, provide functionality that is safe to expose (with permissions) to Non-secure code as

well.

• flash_op() (Arm-S, Arm-NS, RISC-V)
• flash_runtime_to_storage_addr() (Arm-S, Arm-NS, RISC-V)

5.4.6.3. System information

• flash_devinfo16_ptr (Arm-S, RISC-V)
• get_partition_table_info() (Arm-S, Arm-NS RISC-V)
• get_sys_info() (Arm-S, Arm-NS, RISC-V)
• git_revision (Arm-S, Arm-NS, RISC-V)

5.4.6.4. Partition tables

• get_b_partition() (Arm-S, RISC-V)
• get_uf2_target_partition() (Arm-S, RISC-V)
• pick_ab_partition() (Arm-S, RISC-V)
• partition_table_ptr (Arm-S`, RISC-V)
• load_partition_table() (Arm-S, RISC-V)

5.4.6.5. Bootrom memory and state

• set_bootrom_stack() (RISC-V)
• xip_setup_func_ptr (Arm-S, RISC-V)
• bootrom_state_reset() (Arm-S, RISC-V)

5.4.6.6. Executable image management

• chain_image() (Arm-S, RISC-V)
• (explicit_buy() (Arm-S, RISC-V)

5.4.6.7. Security

These Secure-only functions control access for Non-secure code:

• set_ns_api_permission() (Arm-S)
• set_rom_callback() (Arm-S, RISC-V)
• validate_ns_buffer() (Arm-S, RISC-V)

5.4. Bootrom APIs
381

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
