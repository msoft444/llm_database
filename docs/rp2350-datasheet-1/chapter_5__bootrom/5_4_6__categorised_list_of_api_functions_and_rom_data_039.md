---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.4.6. Categorised list of API functions and ROM data
pages: 381-382
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 5.4.6. Categorised list of API functions and ROM data

to claim if the API is concurrently accessed from other contexts. Follow the same steps as the SIO spinlocks (Section
3.1.4) to claim a lock.
The following boot locks are assigned:
• 0x0 : LOCK_SHA_256 - if owned, then a bootrom API is allowed to use the SHA-256 block
• 0x1 : LOCK_FLASH_OP - if owned, then a bootrom API is allowed to enter direct mode on the QSPI memory interface
(Section 12.14.5) in order to perform low-level flash operations
• 0x2 : LOCK_OTP - if owned, then a bootrom API is allowed to access OTP via the serial interface
• 0x7 : LOCK_ENABLE - if owned, then bootrom API resource ownership checking is enabled. This is off by default, since
the bootrom APIs aim to be usable by default without additional setup.
5.4.5. SDK access to the API
Bootrom functions are exposed in the SDK via the pico_bootrom library (see pico_bootrom).
Each bootrom function has a rom_ wrapper function that looks up the bootrom function address and calls it.
The SDK provides a simple implementation of exclusive access via bootrom_acquire_lock_blocking(n) and
bootrom_release_lock(n). When enabled, as it is by default (PICO_BOOTROM_LOCKING_ENABLED=1 is defined) the SDK enables
bootrom locking via LOCK_ENABLE, and these two functions use the other SHA_256/FLASH_OP/OTP boot locks to take ownership
of/release ownership of the corresponding bootrom resource.
The rom_ wrapper functions the SDK call bootrom_acquire_lock_locking and bootrom_relead_lock functions around bootrom
calls that have locking requirements.
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
RP2350 Datasheet
5.4. Bootrom APIs
380


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
RP2350 Datasheet
5.4. Bootrom APIs
381

