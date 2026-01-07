---
layout: post
title: "create a UEFI application in Rust"
excerpt: "create a UEFI application in Rust using uefi crate, run in QEMU."
date: 2026-01-07T15:00:00+0900
categories: [post, os.rs]
tags: [operating-system, os, rust, armv9, uefi]
---

### entering UEFI
modern operating system loading firmware isn't about writing assembly in the master boot record anymore, it's about writing a UEFI application. Intel was the entity to start this standard as "EFI", which was later incorporated into a "UEFI Forum". the [UEFI specification](https://uefi.org/sites/default/files/resources/UEFI_Spec_Final_2.11.pdf) is heavily influenced by the dominance of the C language convention and Microsoft's portable executable format of that era.

we rely on the manufacturer-provided firmware because we want _the manufacturer_ to tell our application about the hardware configuration (e.g. how much memory we have, where is the output, where is the input) at run-time, instead of we _assuming_ or _hard-coding_ the hardware configuration. this firmware then calls our UEFI application using that calling convention.

now at this step, i do not want to write my own implentation of UEFI specification and find out how hardware configurations can be encoded, and then ask the manufacturer (or god forbid, do it myself) to create the firmware with my better standard. so i will use the [`uefi`](https://crates.io/crates/uefi/0.36.1) crate to be able to use existing firmware already created by the manufacturers. the crate helpfully wraps the UEFI into an interface that i can use from Rust. there are many `unsafe` blocks that i hope they have checked 😉.

here is a minimal UEFI application (`uefi-loader/src/main.rs`):
{% highlight rust linenos %}
#![no_main]
#![no_std]

use core::time::Duration;
use log::info;
use uefi::prelude::*;

#[entry]
fn efi_main() -> Status {
	if let Err(e) = uefi::helpers::init() {
		return e.status();
	}
	info!("welcome to os.rs");
	boot::stall(Duration::from_secs(10));
	Status::SUCCESS
}
{% endhighlight %}

it follows the [given example](https://rust-osdev.github.io/uefi-rs/tutorial/app.html), but explicitly handles errors instead of just panicking.

note: the Rust compiler _assumes_ that we want to write an application that is run on top of an existing operating system. so we have to write `#![no_std]` to tell the compiler there is no "standard" operating system library to link; and we have to write `#![no_main]` to tell the compiler that our main function is not the "standard" `fn() -> ()` type it is expecting.

the `#[entry]` macro is provided by the `uefi` crate. it will emit a symbol `efi_main` in the output `.EFI` file that the firmware expects, so that it can "call" (point the program counter to the correct address of) our main function. it also hides the function arguments (`handle`, `system_table`) that the firmware will use to call our function, from our main function; but it will insert these arguments when it rewrites our function.

<details>
<summary>this is roughly how our function is rewritten.</summary>

{% highlight rust %}
// 1. prevent compiler from mangling the name so the UEFI loader can find it.
#[export_name = "efi_main"]
// 2. use the C calling convention as described by the UEFI.
pub extern "C" fn efi_main(image_handle: uefi::Handle, system_table: uefi::table::SystemTable<uefi::table::Boot>) -> uefi::Status {
	// 3. inserts this unsafe block to use image_handle and system_table, among other things.
	unsafe {
		::uefi::boot::set_image_handle(#image_handle_ident);
		::uefi::table::set_system_table(#system_table_ident.cast());
	}
	// 4. proceed with the rest of efi_main() from our definition
}
{% endhighlight %}

</details>


### running it

let's use [QEMU](https://www.qemu.org/) to locally test the prototype for a fast development cycle. testing the latest ARMv9 features requires a specific QEMU flags incantation. i'm using `gic-version=max` for the latest generic interrupt controller, `-cpu max` to enable the latest features like MTE and PAC. the QEMU firmware in this case is in `edk2-aarch64-code.fd`.

{% highlight makefile %}
# executable files
CARGO := /Users/gary/.cargo/bin/cargo
QEMU := /opt/homebrew/bin/qemu-system-aarch64

# paths
TARGET := aarch64-unknown-uefi
EFI_FILE := target/$(TARGET)/debug/uefi-loader.efi
DISK_IMG := target/disk.img
OVMF_PATH := /opt/homebrew/share/qemu/edk2-aarch64-code.fd

# compile
build:
	$(CARGO) build -p uefi-loader --target $(TARGET)

# create a FAT32 disk image containing our EFI file
disk: build
	dd if=/dev/zero of=$(DISK_IMG) bs=1M count=64
	mkfs.fat -F 32 $(DISK_IMG)
	mmd -i $(DISK_IMG) ::/EFI
	mmd -i $(DISK_IMG) ::/EFI/BOOT
	mcopy -i $(DISK_IMG) $(EFI_FILE) ::/EFI/BOOT/BOOTAA64.EFI

# run and test
run: disk
	$(QEMU) \
		-machine virt,gic-version=max \
		-cpu max \
		-m 512M \
		-nographic \
		-bios $(OVMF_PATH) \
		-drive format=raw,file=$(DISK_IMG)
{% endhighlight %}

### result
it boots and prints the welcome message. press `Ctrl+A` then `X` to quit QEMU.

{% highlight text %}
UEFI firmware (version edk2-stable202408-prebuilt.qemu.org built at 16:28:50 on Sep 12 2024)
BdsDxe: loading Boot0001 "UEFI Non-Block Boot Device" from PciRoot(0x0)/Pci(0x2,0x0)
BdsDxe: starting Boot0001 "UEFI Non-Block Boot Device" from PciRoot(0x0)/Pci(0x2,0x0)
[ INFO]: uefi-loader/src/main.rs@013: welcome to os.rs
QEMU 10.2.0 monitor - type 'help' for more information
(qemu) QEMU: Terminated
{% endhighlight %}

<details>
<summary>there may be errors, but we will ignore these because they are not our errors.</summary>

{% highlight text %}
ArmTrngLib could not be correctly initialized.
Error: Image at 0005FD66000 start failed: Not Found
Error: Image at 0005FC8E000 start failed: Unsupported
Error: Image at 0005FC13000 start failed: Not Found
Tpm2SubmitCommand - Tcg2 - Not Found
Tpm2GetCapabilityPcrs fail!
Tpm2SubmitCommand - Tcg2 - Not Found
Image type X64 can't be loaded on AARCH64 UEFI system.
ConvertPages: failed to find range 140000000 - 14000EFFF
{% endhighlight %}

</details>

now that we have written a minimal UEFI application, next let's get the information about the hardware configuration that is passed in to us.
