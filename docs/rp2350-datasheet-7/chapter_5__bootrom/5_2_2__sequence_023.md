---
source_pdf: rp2350-datasheet-7.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.2.2. Sequence
pages: 367-372
type: technical_spec
generated_at: 2026-03-01T06:07:53.318745+00:00
---

# 5.2.2. Sequence

5.2.2. Sequence

This section enumerates the steps of the processor-controlled boot sequence for Arm processors. There are some

minor differences on Arm versus RISC-V, which are discussed in Section 5.2.2.2.

A valid image in Table 451 refers to one that contains a valid block loop, with one of those blocks being a valid image

definition. On a secured RP2350 this image must be (correctly) signed, and must meet all other security requirements

such as minimum rollback version.

Shaded cells in the Action column of Table 451 indicate a boot outcome as described in Section 5.2.1. Other cells are

transitory states which continue through the sequence. Both cores start the sequence at Entry.

The main sequential steps in Table 451 are:

• Entry
• Core 1 Wait
• Boot Path Start
• Await Rescue
• Generate Boot Random
• Check POWMAN Vector
• Check Watchdog Vector
• Prepare for Image Boot
• Try RAM Image Boot
• Check BOOTSEL
• Try OTP Boot
• Try Flash Boot
• Prepare for BOOTSEL
• Enter USB Boot
• Enter UART Boot
• Boot Failure

For a summary of Table 451 in pseudocode form, see Section 5.2.2.1.

| Condition (If…) | Action (Then…) |
| --- | --- |
| Step: Entry |  |
| Always | Check core number in CPUID or MHARTID. |
| Running on core 0 | Clear boot RAM (except for core 1 region and the always region). |
|  | Go to Boot Path Start. |
| Running on core 1 | Go to Core 1 Wait. |
| Step: Core 1 Wait |  |
| Always | Wait for RCP salt register to be marked valid by core 0. |
|  | Wait for core 0 to provide an entry point through Secure SIO FIFO, using the protocol described in Section 5.3. |
|  | Outcome: Set Secure main sp and VTOR, then jump into the entry point provided. |
| Step: Boot Path Start |  |
| Always | Check rescue flag, CHIP_RESET.RESCUE_FLAG |
| Rescue flag set | Go to Await Rescue. |
| Rescue flag clear | Go to Generate Boot Random. |
| Step: Await Rescue |  |
| Always | Clear the rescue flag to acknowledge the request. |
|  | Outcome: Halt in place. The debugger attaches to give the processor further instruction. |
| Step: Generate Boot Random |  |
| Always | Sample TRNG ROSC into the SHA-256 to generate a 256-bit per-boot random number. |
|  | Store 128 bits in boot RAM for retrieval by get_sys_info(), and distribute the remainder to the RCP salt registers. |
|  | Go to Check POWMAN Vector. |
| Step: Check POWMAN Vector |  |
| Always | Read BOOT0 through BOOT3 to determine requested boot type. |
| Boot type parity is valid | Clear BOOT0 so this is ignored on subsequent boots. |
| A BOOTDIS flag is set | Go to Check Watchdog Vector. |
| Boot type is VECTOR | Outcome: Set Secure main sp, then call into the entry point provided. |
| (Return from VECTOR) | Go to Check Watchdog Vector. |
| Other or invalid boot type | Go to Check Watchdog Vector. |
| Step: Check Watchdog Vector |  |
| Always | Read watchdog SCRATCH4 through SCRATCH7 to determine requested boot type. |
| Boot type parity is valid | Clear SCRATCH4 so this is ignored on subsequent boots. |
| Boot type is BOOTSEL | Make note for later: equivalent to selecting BOOTSEL by driving QSPI CSn low. |
| A BOOTDIS flag is set | Go to Prepare for Image Boot (so BOOTSEL is the only permitted type when the OTP BOOTDIS.NOW or POWMAN BOOTDIS.NOW flag is set). |
| Boot type is VECTOR | Outcome: Set Secure main sp, then call into the entry point provided. |
| (Return from VECTOR) | Go to Prepare for Image Boot. |
| Boot type is RAM IMAGE _ | Make note for later: this requests a scan of a RAM region for a preloaded image. |
| Boot type is FLASH UPDATE _ | Make note for later: modifies some flash boot behaviour, as described in Section 5.1.16. |
| Always | Go to Prepare for Image Boot. |
| Step: Prepare for Image Boot |  |
| Always | Clear BOOTDIS flags (OTP BOOTDIS.NOW and POWMAN BOOTDIS.NOW). |
|  | Power up SRAM0 and SRAM1 power domains (XIP RAM domain is already powered). |
|  | Reset all PADS and IO registers, and remove isolation from QSPI pads. |
|  | Release USB reset and clear upper 3 kB of USB RAM (for search workspace). |
|  | Go to Try RAM Image Boot. |
| Step: Try RAM Image Boot |  |
| Watchdog type is not RAM IMAGE _ | Go to Check BOOTSEL. |
| BOOT_FLAGS0.DISABLE_SR AM_WINDOW_BOOT is set | Go to Check BOOTSEL. |
| Otherwise | Scan indicated RAM address range for a valid image (base in SCRATCH2, length in SCRATCH3). This is used to boot into a RAM image downloaded via UF2, for example. |
| RAM image is valid | Outcome: Enter RAM image in the manner specified by its image definition. |
| No valid image | Go to Prepare for Bootsel (skipping flash and OTP boot). |
| Step: Check BOOTSEL |  |
| Always | Check BOOTSEL request: QSPI CSn is low (BOOTSEL button), watchdog type is BOOTSEL, or RUN pin double-tap was detected (enabled by BOOT_FLAGS1.DOUBLE_TAP). |
| BOOTSEL requested | Go to Prepare for BOOTSEL (skipping flash and OTP boot). |
| Otherwise | Go to Try OTP Boot. |
| Step: Try OTP Boot |  |
| Always | Check BOOT_FLAGS0.DISABLE_OTP_BOOT and BOOT_FLAGS0.ENABLE_OTP_BOOT (the disable takes precedence). |
| OTP boot disabled | Go to Try Flash Boot. |
| OTP boot enabled | Load data from OTPBOOT_SRC (in OTP) to OTPBOOT_DST0/OTPBOOT_DST1 (in SRAM), with the length specified by OTPBOOT_LEN. |
|  | Check validity of the image in-place in SRAM. |
| Image is valid | Outcome: Enter RAM image in the manner specified by its image definition. |
| No valid image | Go to Try Flash Boot. |
| Step: Try Flash Boot |  |
| Flash boot disabled by BOOT_FLAGS0 | Go to Prepare for BOOTSEL. |
| Always | Issue XIP exit sequence to chip select 0. |
| FLASH_DEVINFO has GPIO and size for chip select 1 | Issue XIP exit sequence to chip select 1. |
| Always | Scan flash for a valid image (potentially in a partition) with a range of instructions (EBh, BBh, 0Bh, 03h) and SCK divisors (3 to 24) |
| Valid image found | Outcome: Enter flash image in the manner specified by its image definition. This may including loading some flash contents into RAM. Save the current flash read mode as an XIP setup function at the base of boot RAM, which can be called later to restore the current mode (for example, following a serial programming operation). |
| No valid image | Go to Prepare for BOOTSEL. |
| Step: Prepare for BOOTSEL |  |
| Always | Erase SRAM0 through SRAM9, XIP cache and USB RAM to all-zeroes before relinquishing memory and peripherals to Non-secure. |
|  | Enable XOSC and configure PLL for 48 MHz, according to BOOTSEL_XOSC_CFG and BOOTSEL_PLL_CFG (default is to expect a 12 MHz crystal). |
|  | Check QSPI SD1 pin (with default pull-down resistor) for UART/USB boot select. |
|  | Scan flash for a partition table (always using an 03h serial read command with an SCK divisor of 6). The USB bootloader may download UF2s to different flash addresses depending on partitions and their contents. |
|  | Advance all OTP soft locks to the BL state from OTP, if more restrictive than their S state. |
| QSPI SD1 pulled low | Go to Enter USB Boot. |
| QSPI SD1 driven high | Go to Enter UART boot. |
| Step: Enter USB Boot |  |
| Always | Check BOOT_FLAGS0.DISABLE_BOOTSEL_USB_PICOBOOT_IFC and BOOT_FLAGS0.DISABLE_BOOTSEL_USB_MSD_IFC to see which USB interfaces are permitted. |
| Both USB interfaces disabled | Go to Boot Failure. |
| Otherwise | Outcome: Enter USB bootloader. The bootloader reboots if a UF2 image is downloaded, marking a FLASH UPDATE in the watchdog scratch registers if applicable, and the boot path _ restarts from Entry. Valid images boot; invalid images usually end up back in the USB bootloader. |
| Step: Enter UART Boot |  |
| Always | Check BOOT_FLAGS0.DISABLE_BOOTSEL_UART_BOOT to see if UART boot is permitted. |
| UART boot disabled | Go to Boot Failure. |
| Otherwise | Outcome: Enter UART bootloader. The bootloader reboots once an image has been downloaded, with a RAM IMAGE boot type, and the boot path restarts from Entry. Valid _ images boot; invalid images usually end up back in the UART bootloader. |
| Step: Boot Failure |  |
| Always | Outcome: Take no further action. No valid boot image was discovered, and the selected BOOTSEL interface was disabled. Attach the debugger to give the processor further instruction. See the boot reason in boot RAM for diagnostics on why the boot failed. |

Table 451. Processor-

controlled boot

sequence

5.2. Processor-controlled boot sequence
366

RP2350 Datasheet

5.2. Processor-controlled boot sequence
367

RP2350 Datasheet

5.2. Processor-controlled boot sequence
368

RP2350 Datasheet

5.2. Processor-controlled boot sequence
369

RP2350 Datasheet


TIP

The bootrom internally refers to BOOTSEL mode as NSBOOT, because the USB and UART bootloaders run in the

Non-secure state under Arm. This chapter may also occasionally refer to BOOTSEL as NSBOOT.

5.2.2.1. Boot sequence pseudocode

The following pseudocode summarises Table 451.

if (powman_vector_valid && powman_reboot_mode_is_pcsp) {

    // This call may return and continue the boot path

    if (correct_arch) powman_vector_pc(); else hang();

}

if (watchdog_vector_valid) {

    // Make note of RAM_IMAGE, FLASH_UPDATE, BOOTSEL reboot types

    check_special_reboot_mode();

    if (watchdog_reboot_mode_is_pcsp) {

        // This call may return and continue the boot path

        if (correct_arch) watchdog_vector_pc(); else hang();

    }

}

// RAM image window specified by watchdog_scratch, e.g. after a UF2 RAM

// download: either execute the RAM image or fall back to UART/USB boot.

if (watchdog_reboot_mode_is_ram_image && !ram_boot_disabled_in_otp) {

    // This only returns if there is no valid RAM image to enter.

    // You can't return from the RAM image.

    try_boot_ram_image(ram_image_window);

} else {

    // Otherwise try OTP and flash boot (unless there is a request to skip)

    skip_flash_and_otp_boot =

        bootsel_button_pressed() ||

        watchdog_reboot_mode_is_bootsel ||

        (double_tap_enabled_in_otp() && double_run_reset_detected());

    if (!skip_flash_and_otp_boot) {

        if (otp_boot_enabled_in_otp && !otp_boot_disabled_in_otp) {

            // This only returns if there is no valid OTP image to enter.

            // You can't return from the OTP image.

            try_otp_boot();

        }

        if (!flash_boot_disabled_in_otp) {

            // This only returns if there is no valid flash image to enter.

            // You can't return from the flash image.

            try_flash_boot();

        }

    }

}

// Failed to find an image, so drop down into one of the bootloaders

if (sd1_high_select_uart) {

    // Does not return except via reboot

    if (nsboot_uart_disabled) hang(); else nsboot(uart);

} else {

    // Does not return except via reboot

    if (nsboot_usb_disabled) hang(); else nsboot(usb);

}

5.2. Processor-controlled boot sequence
370

RP2350 Datasheet

5.2.2.2. Differences between Arm and RISC-V

The boot sequence outlined in Table 451 has the following differences on RISC-V:

• Secure boot is not supported (from any image source).
• Anti-rollback checking is not supported as it applies only to secure boot.
• Additional security checks such as the use of the RCP to validate booleans are disabled.
• The UART and USB bootloaders continue to run in Machine mode, rather than transitioning from the Arm Secure to

Non-secure state, meaning there is no hardware-enforced security boundary between these boot phases.
• The XIP setup function written to boot RAM on a successful flash boot contains RISC-V rather than Arm

instructions.
