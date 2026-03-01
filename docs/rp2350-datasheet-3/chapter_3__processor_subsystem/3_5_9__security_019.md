---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.5.9. Security
pages: 92-93
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 3.5.9. Security

RP2350 Datasheet

3.5.9. Security

By default, the SWD debug access port allows an external debugger to access all system memory and peripherals, and

to observe and change the execution of software running on the processors. If boot signature enforcement is enabled

(Section 10.1.1), debug access becomes a security concern, as it is able to sidestep this protection. To account for this,

RP2350 supports progressively locking down the debug port using configuration in on-chip OTP storage.

Conceptually there are two control bits: debug disable, and secure debug disable. Debug disable is intended to

completely cut off debug access to the processors and the system bus, whilst the secure debug disable forbids Secure

bus accesses, and halting of processors in the Secure state, but still allows Non-secure software to be debugged as

normal. There are two ways to set these control bits:

• Setting the relevant OTP critical flag: CRIT1.DEBUG_DISABLE or CRIT1.SECURE_DEBUG_DISABLE to set the debug

disable or secure debug disable, respectively
• Installing a 128-bit fixed debug key as OTP key 5 or 6 (Section 3.5.9.2)

OTP configuration changes take effect at the next reset of the OTP block.

Once debug has been disabled, software can re-enable debug using the OTP DEBUGEN register, which allows the secure

and overall debug enable to be cleared individually for each processor. For example, Secure software may implement a

shell where users can authenticate using a cryptographic challenge to enable debug on systems where it is disabled by

default. The DEBUGEN register belongs to the processor cold reset domain, so it is preserved over a PSM reset starting

from as early as OTP (the second PSM stage). This allows almost a full system reset without losing debug access.

To avoid accidental writes of the DEBUGEN register, its bits can be individually locked using the matching bits in

DEBUGEN_LOCK.

This offers increasing levels of debug protection:

1. Fully open: no keys installed and no OTP debug disable flags are set. This is the most convenient configuration for

product development.

2. Access with key only: at least one key is installed, but no OTP debug disable flags are set.

3. No access even with key (an OTP debug disable flag is set), but Secure code can enable debug access by writing

to DEBUGEN.

4. No access even with key (an OTP debug disable flag is set), and DEBUGEN is locked by DEBUGEN_LOCK.

3.5.9.1. Effects of debug disables

The secure debug disable flag (CRIT1.SECURE_DEBUG_DISABLE) has the following effects:

• Set Secure AP enable signals for Arm core 0 and core 1 AHB-APs to 0.

◦This prevents the APs from performing Secure bus accesses (including to the PPB).

◦Status is reported in the SDeviceEn flag of the AHB-AP CSW register.
• Set the Cortex-M33 SPIDEN and SPNIDEN signals for both cores to 0.

◦This prevents the cores from being halted or traced whilst in the Secure state.
• Disable the factory test JTAG interface (Section 10.10).

3.5. Debug
91

RP2350 Datasheet

NOTE

Both AHB-APs' CSW.HNONSEC bits default to 0, generating Secure bus accesses. If the secure debug disable flag is set,

these bits must be set to 1 to generate Non-Secure bus accesses.

The debug disable flag (CRIT1.DEBUG_DISABLE) has all of the effects of the secure debug disable flag. It also has the

following additional effects:

• Set AP enable signals for Arm core 0 and core 1 AHB-APs to 0.

◦This prevents the APs from performing any bus accesses at all (including to the PPB).

◦Status is reported in the DeviceEn flag of the AHB-AP CSW register.
• Set AP enable signal for RISC-V DM APB-AP to 0.

◦This prevents the AP from accessing the RISC-V Debug Module.

◦Status is reported in the DeviceEn flag of the APB-AP CSW register.
• Set DBGEN and NIDEN signals for the CTI to 0.

On RISC-V CRIT1.SECURE_DEBUG_DISABLE has no useful effect. Debug-mode accesses from the cores always have

Secure and Privileged bus attributes, except when reduced by FORCE_CORE_NS. Likewise, System Bus Access via the

Debug Module is always Secure and Privileged, unless FORCE_CORE_NS.CORE1 is set, in which case it is Non-secure

and Privileged. Use the CRIT1.DEBUG_DISABLE flag on RISC-V.

3.5.9.2. Debug keys

Section 13.5.2 describes the OTP hardware access keys. Hardware reads OTP access keys into hidden registers as part

of the OTP power-up sequence which takes place after an OTP reset, and the corresponding OTP locations then

become inaccessible. OTP keys 5 and 6 are special in that they control access to the SWD debug hardware in addition to

functioning as normal OTP page keys.

A debug key is a 128-bit fixed challenge. Installing a debug key in OTP locks down debug access, and it remains locked

until the debug host writes a matching key value through the RP-AP DBGKEY register. This is a write-only interface.

To install a debug key, first program the OTP locations starting from KEY5_0 or KEY6_0. These locations are ECC-

protected. Once you have programmed the 128-bit key value and read it back to confirm the correct value is

programmed, write the raw bit pattern 0x010101 to KEY5_VALID or KEY6_VALID to mark the key as valid. The validity

takes effect at the next reset of the OTP block.

Once a key is valid, the OTP storage locations for that key become inaccessible for both reads and writes. Only the OTP

power-up state machine (Section 13.3.4) can read the key.

The effect of installing debug keys depends on which of key 5 and 6 are installed:

• If key 5 or key 6 is valid, and no matching key (either) has been entered through the RP-AP, all debug is disabled.

This has the same effect as setting CRIT1.DEBUG_DISABLE.
• If key 5 is valid, and no matching key (key 5 specifically) has been entered through the RP-AP, Secure debug is

disabled. This has the same effect as writing CRIT1.SECURE_DEBUG_DISABLE.

When both keys are installed, key 5 provides both Secure and Non-secure debug access, and key 6 provides Non-secure

debug access only. When only a single key is installed, that key provides both Secure and Non-secure debug access.

To enter a key over SWD, first write a 1 to DBGKEY.RESET. Then sequentially write 128 bits to DBGKEY.DATA, each

accompanied by a 1 written to DBGKEY.PUSH. Write the data LSB-first, starting with the lowest-numbered OTP row.

Assuming you wrote a value that matched one of the installed debug keys, debug unlocks after the 128th push. The

SDeviceEn and DeviceEn flags in the Mem-AP CSW registers indicate success or failure.

Failure to supply a matching key through the RP-AP disables debug if it would otherwise be enabled. However, supplying

a key does not enable if it is already disabled for other reasons. For example, if CRIT1.DEBUG_DISABLE is set, and

3.5. Debug
92
