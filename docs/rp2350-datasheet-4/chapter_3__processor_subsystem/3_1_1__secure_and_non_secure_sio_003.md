---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.1.1. Secure and Non-secure SIO
pages: 38-38
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 3.1.1. Secure and Non-secure SIO

![Page 38 figure](images/fig_p0038.png)

3.1.1. Secure and Non-secure SIO

To allow isolation of Secure and Non-secure software, whilst keeping a consistent programming model for software

written to run in either domain, the SIO is duplicated into a Secure and a Non-secure bank. Most hardware is duplicated

between the two banks, including:

• Mailbox FIFOs
• Doorbell registers

3.1. SIO
37
