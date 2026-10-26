# KernelSU-Next vendored source

This directory contains KernelSU-Next as regular files in the alioth kernel
repository; it is no longer a Git submodule. The source is based on upstream
KernelSU-Next commit
[`0b17f24246db5c80c056b92c15bcad2b77f1276e`](https://github.com/KernelSU-Next/KernelSU-Next/commit/0b17f24246db5c80c056b92c15bcad2b77f1276e)
(`Add module preinit: run actions before SELinux policy loads`).

## alioth changes

The upstream source is adapted for this kernel and SUSFS v2.3.0:

- Retains the existing manual KernelSU hooks for this non-GKI kernel.
- Adds SUSFS configuration options in `kernel/Kconfig`.
- Initializes SUSFS from KernelSU and connects SUSFS supercalls.
- Adds SELinux SID helpers used by the SUSFS port.
- Marks eligible zygote child processes for SUSFS handling in the setuid hook.

The kernel-side SUSFS implementation and alioth configuration are maintained
in the parent kernel tree. The `drivers/kernelsu` symlink points to
`KernelSU-Next/kernel` and remains part of the kernel integration.

## Updating

KernelSU-Next is vendored from upstream rather than tracked as a submodule.
To update it, compare a selected upstream commit with this directory and
reapply or review the alioth-specific changes above. Keep the upstream
`LICENSE` file with the source.
