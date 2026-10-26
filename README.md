# Xiaomi alioth Kernel for LineageOS 24

This project builds the Linux kernel for the Xiaomi alioth (SM8250) for use
with LineageOS 24 ([`lineage-24.0`](https://github.com/dchristl/android_kernel_xiaomi_sm8250/tree/lineage-24.0)).
The kernel is based on Linux 4.19.325.

## KernelSU Next

KernelSU Next experiments are maintained on separate branches:

- **[Kprobes branch](https://github.com/dchristl/android_kernel_xiaomi_sm8250/tree/kprobe_kernelsu):**
  experimental tracepoint/kretprobe hooks.
- **[Manual hooks branch](https://github.com/dchristl/android_kernel_xiaomi_sm8250/tree/manually_kernelsu):**
  hooks added directly at the required syscall paths.

This `lineage-24.0` branch does not include KernelSU Next. The two integration
methods are alternatives and should not be enabled together. KernelSU and
Magisk are separate from the audio workaround described below.

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
