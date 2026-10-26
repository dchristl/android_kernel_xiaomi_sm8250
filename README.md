# Xiaomi alioth Kernel for LineageOS 24

This project builds the Linux 4.19.325 kernel for Xiaomi alioth (SM8250),
for use with LineageOS 24 ([`lineage-24.0`](https://github.com/dchristl/android_kernel_xiaomi_sm8250/tree/lineage-24.0)).

## KernelSU Next: Manual hooks

KernelSU Next is integrated on this branch using the manual-hook method
described in the [non-GKI integration guide](https://kernelsu-next.github.io/webpage/pages/how-to-integrate-for-non-gki.html).
The source is pinned to commit
[`0b17f242`](https://github.com/KernelSU-Next/KernelSU-Next/tree/0b17f242)
on the `legacy` branch (`v3.4.0-legacy-21-g0b17f242`).

The alioth configuration enables:

```text
CONFIG_KSU=y
CONFIG_KSU_MANUAL_HOOK=y
```

Manual hooks cover exec/execveat (including compat calls), faccessat, read,
newfstatat/fstatat64, and reboot. Compatibility changes also backport
`path_umount()` and the seccomp `filter_count` field required by this
KernelSU version. KernelSU's kprobe hook mode is not used.

The verified alioth `boot.img` contained the same kernel image as the build
output and embedded the corresponding configuration. KernelSU hook symbols
were also present in the linked `vmlinux`. These checks confirm build
integration only, not runtime behavior on a device.

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

Rebuild and test the kernel against the matching LineageOS build. After an
OTA, use a kernel image built for that exact OS build.
