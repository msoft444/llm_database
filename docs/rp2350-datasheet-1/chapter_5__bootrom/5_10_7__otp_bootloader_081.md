---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.10.7. OTP bootloader
pages: 440-440
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 5.10.7. OTP bootloader

uint32_t partition_info[3];
get_partition_table_info(partition_info, 3, PT_INFO_PARTITION_LOCATION_AND_FLAGS
    | PT_INFO_SINGLE_PARTITION | (boot_partition << 24));
uint16_t first_sector_number = (partition_info[1]
    & PICOBIN_PARTITION_LOCATION_FIRST_SECTOR_BITS)
    >> PICOBIN_PARTITION_LOCATION_FIRST_SECTOR_LSB;
uint16_t last_sector_number = (partition_info[1]
    & PICOBIN_PARTITION_LOCATION_LAST_SECTOR_BITS)
    >> PICOBIN_PARTITION_LOCATION_LAST_SECTOR_LSB;
uint32_t data_start_addr = first_sector_number * 0x1000;
uint32_t data_end_addr = (last_sector_number + 1) * 0x1000;
uint32_t data_size = data_end_addr - data_start_addr;
• get_sys_info() with BOOT_INFO, to get the flash_update_boot_window_base if any:
 uint32_t sys_info[5];
 get_sys_info(sys_info, 5*4, SYS_INFO_BOOT_INFO);
 uint32_t flash_update_boot_window_base = sys_info[3];
• pick_ab_partition() to pick the boot partition between A/B partitions if desired:
uint8_t boot_partition = pick_ab_partition(workarea, 0xC00, 0,
flash_update_boot_window_base);
• or get_b_partition() to find the other partition directly.
• chain_image() with data_start_addr and data_size, to boot a chosen image:
// note a negative 3rd parameter indicates to chain_image that the image is being chanined as
// part of a "flash update" boot, so TBYB and/or version downgrade may be in play
chain_image( workarea,
             0xc00,
            (XIP_BASE + data_start_addr) * (info.boot_type == BOOT_TYPE_FLASH_UPDATE ? -1 :
1),
            data_size
);
NOTE
The workarea used must not overlap the image being chained into, so beware SRAM or packaged binaries. If the
binary overlaps the workarea, the results are undefined, but hardly likely to be good.
5.10.7. OTP bootloader
This is similar to the custom bootloader scenario, but it will be stored in the OTP and will run in SRAM.
One possible use case could place decryption code into OTP which decrypts an executable image from a flash partition
into RAM.
The entire bootloader will need to fit in the OTP rows from 0x0C0 to 0xF48 to avoid interfering with other reserved OTP
functionality, giving a maximum size of 7440 bytes (2 bytes per ECC row). If some boot keys and OTP keys are unused,
this region can extend slightly on either end.
RP2350 Datasheet
5.10. Example boot scenarios
439

