# Xiaomi alioth Kernel for LineageOS 24

This project builds the Linux 4.19.325 kernel for Xiaomi alioth (SM8250),
for use with LineageOS 24 (`lineage-24.0`).

## KernelSU Next: Kprobes and SUSFS

The [`kprobe_kernelsu_susfs` branch](https://github.com/dchristl/android_kernel_xiaomi_sm8250/tree/kprobe_kernelsu_susfs)
integrates KernelSU Next using its tracepoint/kretprobe hook mode and SUSFS
v2.3.0. Manual hook mode remains disabled. The alioth configuration enables
the Kprobe hook and SUSFS path, mount, kstat, uname, logging, symbol hiding,
cmdline/bootconfig, open redirect, and mapped-file features.

The KernelSU Next source is pinned to
[`v3.4.0-legacy-22-g165b87fa`](https://github.com/KernelSU-Next/KernelSU-Next/tree/165b87fa99abfe78058c4dc9df43c432e86b8011).
It is vendored in this repository, with the SUSFS bridge documented in
[`KernelSU-Next/README.md`](KernelSU-Next/README.md). The Kprobe setuid callback
marks eligible Zygote children for SUSFS processing; SUSFS management commands
are routed through KernelSU's existing reboot Kprobe.

The alioth kernel release includes the
`-perf-kernelsu_next_kprobe_susfs_dsp_patch` local version suffix. Automatic
Git suffixing is disabled so the release stays within Linux's 64-character
UTS limit. The compiled-by identity defaults to
`KernelSU_Next@alioth_kprobe_susfs_dsp_patch` and is configured in
`scripts/mkcompile_h`, so the values are versioned with this branch. They can
still be overridden for a local build with `KBUILD_BUILD_USER` and
`KBUILD_BUILD_HOST`:

```sh
KBUILD_BUILD_USER=builder KBUILD_BUILD_HOST=buildhost mka bootimage
```

The [non-GKI integration guide](https://kernelsu-next.github.io/webpage/pages/how-to-integrate-for-non-gki.html)
lists kprobe and manual integration as options and suggests manual hooks if
kprobes do not work or the kernel is older than 4.14. The pinned source's
`kernel/Kconfig` also warns against its kprobe hook mode on kernels below
5.10. This is a warning about KernelSU Next's hook mode, not a claim that
Linux kprobes themselves are unavailable on 4.19. Verify this build on the
device before relying on it.

SUSFS is integrated at kernel level, but configured paths and mounts are not
hidden automatically; a compatible userspace client must set the relevant
rules. This port has not yet been built or runtime-tested on this branch.

KernelSU Next and Magisk are separate components; using Magisk does not
change the kernel's audio-calibration workaround below.

## Analog USB-C headset workaround

For the alioth analog headset path, ADM Audproc calibration records belonging
to module `0x1000a101` are excluded only when the calibration matches the
known block exactly: port `0xb030`, path 0, app type 69940, ACDB ID 10,
48 kHz, and 4780 bytes. The remaining 4292-byte calibration payload is sent.
The original calibration buffer is restored after the send.

This workaround was confirmed to make the headset path usable in device
testing. It is not a confirmed root-cause fix; the cause of the ADSP failure
remains unknown. Other calibration blocks, including Bluetooth audio, do not
match the filter conditions and are not intentionally changed.

No long-term regression testing or kernel build is implied by this
documentation. Rebuild and test the kernel against the matching LineageOS
build. After an OTA, first boot the updated OS, then use a kernel image built
for that exact OS build.
