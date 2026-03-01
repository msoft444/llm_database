---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.4.6. Categorised list of API functions and ROM data
pages: 381-382
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
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
