# Roadmap

This page lists directions that are **not implemented** (or only partially explored). It exists so documentation stays honest.

## Implemented today (reference)

Do not treat these as roadmap items — they already ship:

- BusyBox rootfs + custom `/init`
- Zsh + Oh My Zsh interactive shell
- Initramfs / ext4 disk / GRUB ISO build paths
- QEMU user networking bring-up
- `about`, `hutinfo`, `hutsched`
- Optional `CONFIG_HUTOS_SCHED_LAT` monitor

## Planned / deferred

| Item | Notes | Status |
|------|-------|--------|
| Package manager | opkg/apk-style integration deferred to avoid bloat | **Planned** |
| Limine bootloader | GRUB chosen first; Limine not packaged | **Planned** (optional alternative) |
| OpenRC / BusyBox init | Custom `/init` retained for simplicity | **Planned** evaluation only |
| Richer scheduler tooling | Histograms, ftrace helpers, continuous profiling UI | **Planned** |
| Hardened multi-user login path | Accounts exist; getty login is not the primary demo | **Planned** |
| Automated QEMU CI boots | Path checks exist; full boot CI not required yet | **Planned** |
| Desktop / GUI | Explicitly out of scope for the minimal OS | **Not planned** near-term |

## Experimental

| Item | Notes |
|------|-------|
| ISO + mdev on CD-ROM | Coldplug is timed to avoid hangs; edge cases may remain |
| Scheduler latency numbers under QEMU | Useful for demos; not bare-metal SLAs |

## How roadmap items graduate

1. Design note or issue
2. Feature branch + tests under QEMU
3. Documentation update in **this** repository with status **Implemented**

## See also

- [Architecture](architecture.md)
- [Contributing](contributing.md)
