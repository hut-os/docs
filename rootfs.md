# Root filesystem (`rootfs/`)

Status: **Implemented** layout used to build initramfs and disk images.

## Purpose

`rootfs/` is the source tree for the HUT OS userspace image. Build scripts package it into:

- `build/initramfs.cpio.gz`
- `build/hutos.img` (ext4, staged copy of rootfs + `/boot` kernel)
- ISO boot path uses the initramfs (GRUB loads kernel + initrd)

## Top-level layout

```text
rootfs/
├── init                 # PID 1 script
├── bin/                 # BusyBox, zsh, HUTOS tools, applet symlinks
├── sbin/                # admin applet symlinks + helpers
├── etc/                 # passwd, hostname, mdev, network, banner, os-release
├── root/                # root home (.zshrc, .oh-my-zsh)
├── home/hut/            # secondary user home
├── lib64/               # dynamic linker for Zsh
├── usr/lib/…            # Zsh shared libraries and modules
├── usr/share/terminfo/  # terminal definitions
├── proc/ sys/ dev/ tmp/ run/ var/ mnt/ media/
└── …
```

## Important files

| Path | Role |
|------|------|
| `/init` | Boot sequence |
| `/bin/busybox` | Core toolkit |
| `/bin/zsh` | Interactive shell |
| `/bin/about` | Project / developer information |
| `/bin/hutinfo` | Runtime system summary |
| `/bin/hutsched` | Scheduler monitor helper |
| `/bin/hutsched-bench` | Scheduler microbenchmark helper (testing) |
| `/etc/hut-banner.txt` | ASCII banner shown at boot |
| `/etc/mdev.conf` | BusyBox mdev rules |
| `/etc/network/interfaces` | ifup/ifdown config (lo, eth0 dhcp) |
| `/etc/os-release` | Distribution metadata |

## What is **not** in rootfs

- Full Linux kernel source
- Package manager databases
- Desktop environments
- Network-required Oh My Zsh update services

## Rebuilding after edits

```bash
# From hut-os checkout
./build.sh initramfs
./run.sh
```

Disk/ISO:

```bash
./build.sh all
```

## See also

- [Userspace](userspace.md)
- [Building](building.md)
- [System tools](system-tools.md)
