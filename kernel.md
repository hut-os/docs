# Kernel

## Overview

HUT OS runs a **standard Linux kernel** built from upstream sources. The project publishes:

- A defconfig: [`hutos_defconfig`](https://github.com/hut-os/linux-config/blob/main/hutos_defconfig)
- Optional patches under [linux-config/patches](https://github.com/hut-os/linux-config/tree/main/patches)

The full kernel tree is **not** vendored in [hut-os/hut-os](https://github.com/hut-os/hut-os) (build artifacts are large; upstream Linux remains the source of truth).

Status: **Implemented** configuration and build workflow.

## Documented target

Documentation and patches have been validated against **Linux 7.3-rc5**. Newer tags may work after `make olddefconfig`, but are not automatically guaranteed.

Localversion branding (in defconfig):

```text
CONFIG_LOCALVERSION="-HUTOS-Hamedan-University-of-Technology"
```

Example `uname -r`:

```text
7.3.0-rc5-HUTOS-Hamedan-University-of-Technology
```

## Notable config options (non-exhaustive)

| Option | Role |
|--------|------|
| `CONFIG_EXT4_FS=y` | Persistent disk root |
| `CONFIG_DEVTMPFS=y` | Kernel-populated `/dev` |
| `CONFIG_VIRTIO_BLK=y` / `CONFIG_VIRTIO_NET=y` | QEMU virtio |
| `CONFIG_E1000=y` | QEMU e1000 NIC (user networking) |
| `CONFIG_SCHEDSTATS=y` / `CONFIG_SCHED_INFO` | Upstream scheduler stats infrastructure |
| `CONFIG_HUTOS_SCHED_LAT=y` | HUTOS wait-latency monitor (see [scheduler.md](scheduler.md)) |
| `CONFIG_PROC_FS=y` / `CONFIG_SYSCTL` | `/proc`, sysctl for HUTOS tools |
| `CONFIG_DEBUG_FS=y` / `CONFIG_FTRACE=y` | Available in current defconfig; not required for `hutsched` |

## Build (host)

```bash
git clone https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git
cd linux
git checkout v7.3-rc5

# Optional but recommended for HUTOS scheduler feature:
git am /path/to/linux-config/patches/0001-sched-add-HUTOS-run-queue-wait-latency-monitor.patch

cp /path/to/linux-config/hutos_defconfig .config
# or: cp /path/to/hut-os/configs/hutos_defconfig .config

make olddefconfig
make -j"$(nproc)" bzImage
```

Install into the HUT OS tree:

```bash
mkdir -p /path/to/hut-os/kernel/arch/x86/boot
cp arch/x86/boot/bzImage /path/to/hut-os/kernel/arch/x86/boot/bzImage
```

## Runtime expectations

- Boot scripts default to `kernel/arch/x86/boot/bzImage`.
- Initramfs and disk/ISO builds do not recompile the kernel; they package userspace around an existing `bzImage`.

## See also

- [Scheduler](scheduler.md)
- [Building](building.md)
- [Boot](boot.md)
