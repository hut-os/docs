# Architecture

## Summary

HUT OS is a **small Linux distribution**, not a from-scratch kernel. It combines:

- Upstream **Linux** with a HUTOS-branded configuration (and optional patches)
- A BusyBox-based **root filesystem**
- A custom **`/init`** script
- **Zsh** + a minimal **Oh My Zsh** install as the interactive shell
- Optional **GRUB** ISO and **ext4** disk images for more realistic boot paths

## High-level diagram

```text
GRUB (ISO)  ──or──  QEMU -kernel (dev shortcut)
        │
        ▼
   Linux Kernel
   (upstream + hutos_defconfig
    + optional HUTOS-SCHED patch)
        │
        ▼
   /init  (BusyBox ash)
        │
        ├─ mount proc/sys/dev/pts/tmp/run
        ├─ mdev coldplug (timed)
        ├─ hostname
        ├─ lo + eth0/DHCP (best-effort)
        ├─ HUT banner + welcome
        └─ Zsh (+ Oh My Zsh)
                │
                ▼
        about · hutinfo · hutsched · BusyBox tools
```

## Component map

| Layer | Implementation | Status |
|-------|----------------|--------|
| Kernel | Upstream Linux + `configs/hutos_defconfig` | **Implemented** |
| Scheduler policy | Linux EEVDF/fair class (stock) | **Implemented** (upstream) |
| Scheduler observability | `CONFIG_HUTOS_SCHED_LAT` | **Implemented** (optional; off by default) |
| Bootloader | GRUB2 on ISO | **Implemented** |
| Persistent root | ext4 disk image | **Implemented** |
| Init | Custom `/init` (not systemd/OpenRC) | **Implemented** |
| Devices | `devtmpfs` + BusyBox **mdev** | **Implemented** |
| Networking | lo + eth0 + `udhcpc` (QEMU user net) | **Implemented** |
| Shell | Zsh 5.9 + Oh My Zsh (minimal) | **Implemented** |
| Package manager | — | **Planned** / deferred |

## Design principles

1. **Reuse mature projects** (Linux, BusyBox, Zsh, GRUB) instead of reinventing them.
2. Keep the system **small** and bootable in QEMU for demos.
3. Prefer **optional, gated** kernel extensions (static keys) over hot-path cost.
4. Keep **university identity** visible (`about`, banner, hostname `hut-os`) without claiming to be a commercial OS.

## What HUT OS is not

- Not a desktop distribution
- Not a replacement for the Linux scheduler
- Not a full userspace with package repositories (yet)

## Related pages

- [Kernel](kernel.md)
- [Boot](boot.md)
- [Userspace](userspace.md)
- [Roadmap](roadmap.md)
