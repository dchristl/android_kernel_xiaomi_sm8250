Linux kernel
============

There are several guides for kernel developers and users. These guides can
be rendered in a number of formats, like HTML and PDF. Please read
Documentation/admin-guide/README.rst first.

In order to build the documentation, use ``make htmldocs`` or
``make pdfdocs``.  The formatted documentation can also be read online at:

    https://www.kernel.org/doc/html/latest/

There are various text files in the Documentation/ subdirectory,
several of them using the Restructured Text markup notation.
See Documentation/00-INDEX for a list of what is contained in each file.

Please read the Documentation/process/changes.rst file, as it contains the
requirements for building and running the kernel, and information about
the problems which may result by upgrading your kernel.

Alioth: KernelSU Next
=====================

KernelSU Next is integrated into this kernel for the Xiaomi alioth (SM8250)
using the manual-hook method described in the KernelSU Next non-GKI guide.
The integrated source is pinned at commit ``0b17f242`` on the ``legacy``
branch (``v3.4.0-legacy-21-g0b17f242``).

The KernelSU source is linked from ``KernelSU-Next/kernel`` into
``drivers/kernelsu``. ``drivers/Kconfig`` and ``drivers/Makefile`` include
the KernelSU Kconfig and build rules. The alioth config fragment enables:

.. code-block:: text

   CONFIG_KSU=y
   CONFIG_KSU_MANUAL_HOOK=y

Manual hooks are added for exec/execveat (including compat calls), faccessat,
read, newfstatat/fstatat64, and reboot. The KernelSU build also backports
``path_umount()`` and the seccomp ``filter_count`` field required by this
KernelSU version.

The verified alioth ``boot.img`` contains the same kernel image as the build
output and embeds the configuration above. KernelSU hook symbols were also
present in the linked ``vmlinux``. The final manual-hook configuration has
``CONFIG_KPROBES`` disabled; it does not use KernelSU's kprobe hook. These
checks confirm build integration only, not runtime behavior on a device.
