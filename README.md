# Xiaomi alioth Kernel for LineageOS 24

This project builds the Linux 4.19.325 kernel for Xiaomi alioth (SM8250),
for use with LineageOS 24 (`lineage-24.0`).

## KernelSU Next: Kprobes

This branch, [`kprobe_kernelsu`](https://github.com/dchristl/android_kernel_xiaomi_sm8250/tree/kprobe_kernelsu),
integrates KernelSU Next using its tracepoint/kretprobe hook mode. The alioth
configuration enables `CONFIG_KSU`, `CONFIG_KPROBES`, `CONFIG_KPROBE_EVENTS`,
and `CONFIG_KSU_KPROBES_HOOK`; manual hook mode is disabled.

The KernelSU Next source is pinned to
[`v3.4.0-legacy-21-g0b17f242`](https://github.com/KernelSU-Next/KernelSU-Next/tree/0b17f242).
For the alternative manual-hook integration, see the
[`manually_kernelsu` branch](https://github.com/dchristl/android_kernel_xiaomi_sm8250/tree/manually_kernelsu).

The [non-GKI integration guide](https://kernelsu-next.github.io/webpage/pages/how-to-integrate-for-non-gki.html)
lists kprobe and manual integration as options and suggests manual hooks if
kprobes do not work or the kernel is older than 4.14. The pinned source's
`kernel/Kconfig` also warns against its kprobe hook mode on kernels below
5.10. This is a warning about KernelSU Next's hook mode, not a claim that
Linux kprobes themselves are unavailable on 4.19. Verify this build on the
device before relying on it.
