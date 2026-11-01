# SukiSU Ultra main kernel sources

The kernel driver and UAPI are vendored from the official `main` branch at
commit `56d45a19193a41bf42c8db655c9d2cfba44c48f1`.

This integration follows the upstream `main` branch setup: `CONFIG_KSU=y`
with Kprobe-based syscall hooks. No manual syscall hooks or SUSFS kernel-core
patches are included in this branch.
