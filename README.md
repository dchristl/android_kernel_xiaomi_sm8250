# Xiaomi alioth Kernel for LineageOS 24

This project builds the Linux kernel for the Xiaomi alioth (SM8250) for use
with LineageOS 24 ([`lineage-24.0`](https://github.com/dchristl/android_kernel_xiaomi_sm8250/tree/lineage-24.0)).
The kernel is based on Linux 4.19.325.

## SukiSU Ultra and SUSFS

The `sukisu_susfs` branch integrates SukiSU Ultra v4.3.0 with kprobe-based
syscall hooks and SUSFS v2.3.0. SukiSU's kernel source is vendored under
[`SukiSU-Ultra/kernel`](SukiSU-Ultra/kernel), with its upstream revision and
local integration changes documented in
[`SukiSU-Ultra/UPSTREAM.md`](SukiSU-Ultra/UPSTREAM.md).

SUSFS path, mount, kstat, uname, cmdline, open-redirect, and mapped-file
features are enabled in the alioth configuration. Kernel support alone does
not add hide rules; a compatible SukiSU/SUSFS userspace module must configure
them.

The Linux 4.19 SELinux port applies KernelSU rules to a cloned policy database
and swaps it under the SELinux policy lock. SukiSU's dynamic SELinux policy
batch API returns `-EOPNOTSUPP` on this kernel, and the optional `selinux_hide`
feature is disabled because it depends on newer SELinux policy structures.
The SukiSU driver and modified kernel objects compile; a full image and device
runtime have not been tested.

## Analog USB-C headset workaround

For the alioth analog headset path, ADM Audproc calibration records belonging
to module `0x1000a101` are excluded only when the calibration matches the
known block exactly: port `0xb030`, path 0, app type 69940, ACDB ID 10,
48 kHz, and 4780 bytes. The remaining 4292-byte calibration payload is sent.
The original calibration buffer is restored after the send.

This workaround was confirmed to make the headset path usable in device
testing. It is not a confirmed root-cause fix; the cause of the ADSP failure
remains unknown. Other calibration blocks and Bluetooth audio do not match
the filter conditions, so they are not intentionally changed.

No long-term regression testing or kernel build is implied by this
documentation. Rebuild and test the kernel against the matching LineageOS
build. After an OTA, use a kernel image built for that new OS build.
