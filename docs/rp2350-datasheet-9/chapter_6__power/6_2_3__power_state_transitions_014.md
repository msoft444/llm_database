---
source_pdf: rp2350-datasheet-9.pdf
repository: llm_database
chapter: Chapter 6. Power
section: 6.2.3. Power state transitions
pages: 446-449
type: technical_spec
generated_at: 2026-03-01T11:44:31.548839+00:00
---

# 6.2.3. Power state transitions

6.2.3. Power state transitions

Transitions between power states can be initiated by software, hardware, or via the chip’s debug subsystem. After

initiation, transitions are managed by autonomous power sequencers in the chip’s AON power domain. The power

sequencers can be configured, in a limited way, via the SEQ_CFG register. The sequencers can also be observed and

controlled, again in a limited way, via the RP-AP registers in the chip’s debug subsystem. These registers are described

in Section 3.5.10.

Valid power state transitions are as follows:

• all transitions from one P0.m state (switched core powered on) to another P0.m state (switched core powered on), if

they increase or decrease the number of SRAM domains that are powered on
• all transitions from a P0.m state (switched core powered on) to a P1.m state (switched core powered off), except

transitions that would result in a powered off SRAM domain becoming powered on
• all transitions from a P1.m state (switched core powered off) to a P0.m state (switched core powered on), except

transitions that would result in a powered on SRAM domain becoming powered off

Transitions from one P1.m state (switched core powered off) to another P1.m state (switched core powered off) are not

supported, and will be prevented by the hardware.

Valid transitions are shown in the table below.

![Page 446 figure](images/fig_p0446.png)

*Table 476. valid power state transitions From To*

6.2. Power management
445

RP2350 Datasheet

![Page 447 figure](images/fig_p0447.png)

6.2.3.1. Transitions from Normal Operating (P0.m) states

Transitions from a Normal Operating (P0.m) state to either a Low Power (P1.m) state, or another Normal Operating (P0.m)

state, are initiated by writing to the STATE.REQ field. REQ is a 4-bit field representing the requested power state of the

switched core and memory power domains. The STATE.WAITING field will be set immediately, followed by the the

STATE.CHANGING field, after the actual state change starts. If a transition to a Low Power (P1.m) state is requested,

WAITING will remain set until the processors have gone into a low power state (via __wfi()). In the WAITING state, writing to

the STATE.REQ field can change or cancel the initial request. The requested state can’t be changed when in the CHANGING

state.

A request to move to an unsupported state, or a state that would result in an invalid transition, causes the

STATE.BAD_SW_REQ field to be set.

If a hardware power up request is received while in the WAITING state, the transition requested via STATE.REQ will be

halted and the power up request completed. The STATE.PWRUP_WHILE_WAITING and STATE.REQ_IGNORED fields will

be set.

On writing to STATE.REQ:

• If there is a pending power up request, STATE.REQ_IGNORED is set and no further action is taken
• If the requested state is invalid, STATE.BAD_SW_REQ is set and no further action is taken
• If the switched core is being powered off, STATE.WAITING is set until both processors enter __wfi(). After which

STATE.CHANGING will be set, but no processors will be powered up to read the flag at this time

◦If there is a power up request while in STATE.WAITING, STATE.PWRUP_WHILE_WAITING is set, which can

also raise an interrupt to bring the processors out of __wfi(). No further action is taken

◦You can get out of the WAITING state by writing a new request to STATE.REQ before both processors have gone

into __wfi()
• Any state request that isn’t powering down the switched core, such as powering up or down SRAM domain 0 or 1

starts immediately. Software should wait until STATE.CHANGING has cleared to know the power down sequence.

After the STATE.CHANGING flag is cleared STATE.CURRENT is updated.
• If powering up, software should also wait for STATE.CHANGING to make sure everything is powered up before

continuing. In practice this is handled by the RP2350 bootrom.

Invalid state transitions are:

• any combination of power up and power down requests
• any request which would result in power down of XIP/bootRAM and power up of SWCORE

If XIP, boot RAM, sram0, or sram1 remain powered while SWCORE is powered off, the sram will automatically switch to

a low power state. Stored data will be retained.

Before transitioning to a switched-core power down state (P1.m), software needs to configure:

• the GPIO wakeup conditions if required
• the wakeup alarm if required
• the return state of the SRAM0 & SRAM1 domains

6.2. Power management
446

RP2350 Datasheet

6.2.3.2. Transitions from Low Power (P1.m) states

Transitions from P1.m to P0.m states are initiated by GPIO events or the timer alarm.

There are up to 5 wakeup sources:

• up to 4 GPIO wakeups (level high/low or falling edge/rising edge)
• 1 alarm wakeup

GPIO wakeups are configured by the PWRUP0-PWRUP3 registers. The wakeups are not enabled until the power

sequencer completes the power down operation.

The alarm wakeup is configured by writing to the ALARM_TIME_15TO0-ALARM_TIME_63TO48 registers. The alarm

wakeup has a resolution of 1ms. Once set, the alarm wakeup is armed by writing a 1 to both TIMER.PWRUP_ON_ALARM

and TIMER.ALARM_ENAB. If the alarm fires during the power down sequence, a power up sequence will start when the

power down sequence completes.

The LAST_SWCORE_PWRUP register indicates which event caused the most recent power up.

6.2.3.3. Debugger-initiated power state transitions

The debugger can be used to trigger a power up sequence via the CSYSPWRUPREQ output from the SW-DP CTRL/STAT register.

This powers all domains (i.e. returns to state P0.0) and also inhibits any further software initiated power state

transitions.

When CSYSPWRUPREQ is asserted, the power sequencer will:

• complete any power state transitions that are in progress
• return to power state P0.0
• assert CSYSPWRUPACK to signal completion to the debug host

If CSYSPWRUPREQ is de-asserted then software initiated power transitions will be able to resume. The user can detect when

a software requested transition is ignored because of CSYSPWRUPREQ using the following hints:

• Getting a STATE.REQ_IGNORED after a write to STATE.REQ
• CURRENT_PWRUP_REQ will have bit 5 (coresight) set
• Either:

◦Get the debugger to de-assert CSYSPWRUPREQ or

◦Mask out CSYSPWRUPREQ by setting DBG_PWRCFG.IGNORE

NOTE

DBG_PWRCFG.IGNOREis useful to test going to sleep with a debugger attached or ignoring CSYSPWRUPREQ. A debugger

will likely leave CSYSPWRUPREQ set when disconnecting. It would be impossible to go to sleep after this without

DBG_PWRCFG.IGNORE.

6.2.3.4. Power-mode-aware GPIO control

The power manager sequencer is able to switch the state of two GPIO outputs on entry to and exit from a P1.m state, i.e.

one where the switched core is powered down. This allows external devices to be power-aware. The GPIOs switch to

indicate the low power state after the core is powered down and switch to indicate the high power state before the core

is powered up. This ensures the high power state of the external components always overlaps the high power state of

the core. The GPIOs are configured by the EXT_CTRL0 and EXT_CTRL1 registers.

6.2. Power management
447

RP2350 Datasheet

6.2.3.5. Isolation

When powering down SWCORE, the pad control and data signals are latched and isolated from the IO logic. This avoids

transitions on pads which could potentially corrupt external components. On SWCORE power up, the isolation is not

released automatically. The user releases the isolation by clearing the ISO field of the pad control register (for example

GPIO0.ISO) after the IO logic has been configured.
