# SukiSU Ultra kernel sources

The `kernel/` directory is vendored from
[SukiSU-Ultra/SukiSU-Ultra](https://github.com/SukiSU-Ultra/SukiSU-Ultra)
tag `v4.3.0`, commit
`e67b14e6bbbbd3e67f24df658a01d3e73a64159c`.

Local changes add the SUSFS v2.3.0 Kconfig options, initialization, setuid
process marking, and reboot-supercall dispatch needed by this kernel port.
The upstream kernel license is retained in `kernel/LICENSE`.
