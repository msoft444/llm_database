---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.10.2. Signed images
pages: 431-433
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 5.10.2. Signed images

$ picotool seal --sign unsigned.elf signed.elf private.pem /path/to/otp.json
To configure the SDK to output this OTP JSON file when signing, add the following command to your CMakeLists.txt:
pico_set_otp_key_output_file(target_name /path/to/otp.json)
You can then issue the following command to write this OTP JSON file to the device, thus enabling secure boot:
$ picotool otp load /path/to/otp.json
Once secure boot is enabled, the bootrom verifies signatures of images from all supported media: flash, OTP, and
images preloaded into SRAM via the UART and USB bootloaders. At this point you lose the ability to run unsigned
images; during development you may find it more convenient to leave secure boot disabled. The next section describes
the generation of signed images to run on a secure-boot-enabled device.
5.10.2. Signed images

TIP
This section refers to the concepts of block loops and image definitions (and the associated IMAGE_DEF data
structure) described in Section 5.1. You should read the bootrom concepts section before this one.
An example of an image (and its block loop) produced by the SDK is shown below.
RP2350 Datasheet
5.10. Example boot scenarios
430


Block 
Loop
IMAGE_DEF Item
IGNORED Item
Vector table
Data
Initial Metadata Block 
(must be in first 4kB)
Empty Block
(placed at the end by 
default to catch 
overwrite at the end 
of the binary)
Code
The first block must be within the first 4 kB of the image, and is an IMAGE_DEF block describing the image. This block is
linking to an empty block at the end of the image, that is included in the block loop to help detect partially written
binaries. If the end of the image is missing or overwritten, then the block loop not be properly closed and will be
considered invalid.
picotool can be used to sign a binary, in which case it modifies the image as follows:
RP2350 Datasheet
5.10. Example boot scenarios
431


IMAGE_DEF Item
IGNORED Item
LOAD_MAP Item
HASH_DEF Item
SIGNATURE Item
HASH_VALUE Item
Block 
Loop
LOAD_MAP
entry covers 
this region
or
IMAGE_DEF Item
(this will be 
superceded by later 
IMAGE_DEF in the 
block loop)
Vector table
Data
Signature Block
Initial Metadata Block 
(must be in first 4kB)
Code
Note that the marker block at the end of the image has been replaced with a new IMAGE_DEF block including the first
block’s information along with additional new information. The new information includes the signature (or hash value if
hashing only), along with a LOAD_MAP entry indicating the regions of the image that are signed or hashed.
At runtime, the bootrom will pick the last valid IMAGE_DEF in the block loop as the one to boot.
Signing requires a SHA-256 hash of the data specified in the LOAD_MAP, along with the words of the block specified by the
HASH_VALUE Item (which must include the first word of the SIGNATURE Item). This hash is then signed with an ECDSA
secp256k1 private key, to produce the 64 byte signature stored in the SIGNATURE Item.
For secure boot, it is recommended to use packaged SRAM binaries instead of flash binaries, as the signature check is
only performed during boot, so a malicious actor with physical access could replace the data on the external flash after
the signature check to run unsigned code.
To sign and/or hash a binary in the SDK, you can add the following functions to your CMakeLists.txt file:
pico_sign_binary(target_name /path/to/keyfile.pem)
pico_hash_binary(target_name)
This will invoke the picotool seal command to sign and/or hash your binary when you call pico_add_extra_outputs. You
can manually invoke picotool seal to sign and/or hash a binary using:
$ picotool seal --sign --hash unsigned.elf signed.elf private.pem
RP2350 Datasheet
5.10. Example boot scenarios
432

