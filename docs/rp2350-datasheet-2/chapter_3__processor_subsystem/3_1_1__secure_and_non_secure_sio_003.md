---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.1.1. Secure and Non-secure SIO
pages: 38-38
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 3.1.1. Secure and Non-secure SIO

![Page 38 figure](images/fig_p0038.png)

RP2350 Datasheet

Figure 7. The single-

cycle IO block

Core 0

Core 1

contains registers

Load/Store

Load/Store

which processors

must access quickly.

FIFOs, doorbells and

S           NS
S           NS

spinlocks support

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|  |  |  |  |  | Non-secure SIO |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | Bus
nterface |
| Secure SIO
CPUID 0 CPUID 1
CPUID 0 CPUID 1
FIFO 4 × 32b
FIFO 4 × 32b
FIFO 4 × 32b
Bus FIFO 4 × 32b
Hardware Spinlock × 32
Interface I
Bus Bus
Hardware Spinlock × 32
Interface Doorbells × 8 Each Way Interface
Doorbells × 8 Each Way
RISC-V Platform Timer
RISC-V Platform Timer |  |  | Secure SIO
CPUID 0 CPUID 1
CPUID 0 CPUID 1
FIFO 4 × 32b
FIFO 4 × 32b
FIFO 4 × 32b
Bus FIFO 4 × 32b
Hardware Spinlock × 32
Interface I
Bus
Hardware Spinlock × 32
ace Doorbells × 8 Each Way Interface
Doorbells × 8 Each Way
RISC-V Platform Timer
RISC-V Platform Timer |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
|  |  |  |  | Bus
Interface |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
|  | Bus
Interface |  | ace |  |  |  | CPUID 0 CPUID 1
FIFO 4 × 32b |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |  |  | D 0 CPUID 1
FIFO 4 × 32b |  |  | CPUID 1
FIFO 4 × 32b |  |  |  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
|  |  |  |  |  |  |  | FIFO 4 × 32b |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |  |  | FIFO 4 × 32b |  |  |  |  |  |  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
|  |  |  |  |  |  |  | FIFO 4 × 32b |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |  |  | FIFO 4 × 32b |  |  |  |  |  |  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
|  |  |  |  |  |  |  | Hardware Spinlock × 32 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |  |  | rdware Spinlock × 32 |  |  |  |  |  |  |  |  | Int |  |  |  |
|  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
|  |  |  |  |  |  |  | Doorbells × 8 Each Way |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |  |  | orbells × 8 Each Way |  |  |  |  |  |  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
|  |  |  |  |  |  |  | RISC-V Platform Timer |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
|  |  |  |  |  |  |  | RISC-V P |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
|  |  |  | Interp0 Interp1 TMDS TMDS Interp1 Interp0
(S/NS) (S/NS) (S/NS) (S/NS) (S/NS) (S/NS) |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
|  |  |  |  | Interp0 I
(S/NS) ( |  |  | nterp1
S/NS) |  | TMDS
(S/NS) |  |  | TMDS I
(S/NS) |  | nterp1 I
(S/NS) ( |  | nterp0
S/NS) |  |  |  |  |  |

message passing and

synchronisation

between the two

cores. The shared

GPIO registers provide

fast, direct access to

GPIO-capable pins.

Interpolators can

accelerate common

software tasks. Most

SIO hardware is

banked (duplicated)

for Secure and Non-

secure access. Grey

arrows show bus

connections for Non-

secure access.

GPIO Registers (Shared S + NS)

PIO × | 48 +

To IO Muxing

The SIO contains:

• CPUID registers which read as 0/1 on core 0/1 (Section 3.1.2)
• Mailbox FIFOs for passing ordered messages between cores (Section 3.1.5)
• Doorbells for interrupting the opposite core on cumulative and unordered events (Section 3.1.6)
• Hardware spinlocks for implementing critical sections without using exclusive bus accesses (Section 3.1.4)
• Interpolators (Section 3.1.10) and TMDS encoders (Section 3.1.9)
• Standard RISC-V 64-bit platform timer (Section 3.1.8) which is usable by both Arm and RISC-V software
• GPIO registers for fast software bitbanging (Section 3.1.3), with shared access from both cores

Most SIO hardware is duplicated for Secure/Non-secure access. Non-secure access to the FIFO registers will see a

physically different FIFO than Secure access to the same address, so that messages belonging to Secure and Non-

secure software are not mixed: Section 3.1.1 describes this Secure/Non-secure banking in more detail.

3.1.1. Secure and Non-secure SIO

To allow isolation of Secure and Non-secure software, whilst keeping a consistent programming model for software

written to run in either domain, the SIO is duplicated into a Secure and a Non-secure bank. Most hardware is duplicated

between the two banks, including:

• Mailbox FIFOs
• Doorbell registers

3.1. SIO
37
