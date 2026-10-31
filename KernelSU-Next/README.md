# KernelSU-Next vendored source

This directory contains KernelSU-Next as regular files in the alioth kernel
repository rather than as a Git submodule. The source is based on upstream
commit
[`165b87fa99abfe78058c4dc9df43c432e86b8011`](https://github.com/KernelSU-Next/KernelSU-Next/commit/165b87fa99abfe78058c4dc9df43c432e86b8011)
(`legacy: fix build errors on Linux 4.9 kernels`), version
`v3.4.0-legacy-22-g165b87fa`.

The alioth integration preserves KernelSU's tracepoint/kretprobe hook mode.
SUSFS v2.3.0 is connected through KernelSU initialization, SELinux SID helpers,
the existing setuid callback, and the existing reboot-Kprobe supercall path.
The kernel-side SUSFS port and enabled alioth options are maintained in the
parent repository.

To update KernelSU, compare a selected upstream commit with this vendored
source and recheck the alioth-specific SUSFS integration. Keep the upstream
license files with the source.
