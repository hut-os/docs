# HUT OS Documentation

**Official documentation for HUT OS — Hamedan University of Technology Operating System**

HUT OS is a minimal, educational Linux-based operating system built to be understandable, hackable, and suitable for university OS coursework and demos.

This repository contains the **canonical Markdown documentation**. Source code lives in sibling repositories under the [hut-os](https://github.com/hut-os) organization.

---

## Status legend

Documentation and feature lists use these labels:

| Label | Meaning |
|-------|---------|
| **Implemented** | Present in the current HUT OS tree and exercised under QEMU |
| **Experimental** | Present, but optional, gated, or not yet heavily hardened |
| **Planned** | Intended direction; not implemented yet |

---

## Documentation index

| Page | Description |
|------|-------------|
| [Getting started](getting-started.md) | Clone, build prerequisites, first boot |
| [Architecture](architecture.md) | High-level design and component map |
| [Kernel](kernel.md) | Upstream Linux + HUT OS configuration |
| [Scheduler](scheduler.md) | HUTOS run-queue wait latency monitor |
| [Userspace](userspace.md) | BusyBox, Zsh, Oh My Zsh, accounts |
| [Root filesystem](rootfs.md) | Layout of `rootfs/` |
| [Boot](boot.md) | Initramfs, disk, and GRUB ISO paths |
| [Networking](networking.md) | Loopback, eth0, DHCP under QEMU |
| [System tools](system-tools.md) | `about`, `hutinfo`, `hutsched`, BusyBox |
| [Development](development.md) | Local workflow and repository layout |
| [Building](building.md) | `./build.sh` and image artifacts |
| [Testing](testing.md) | QEMU checks and scheduler smoke tests |
| [Contributing](contributing.md) | Branches, commits, pull requests |
| [Roadmap](roadmap.md) | Planned work (explicitly not done yet) |

---

## Related repositories

| Repository | Role |
|------------|------|
| [hut-os/hut-os](https://github.com/hut-os/hut-os) | Main distribution: rootfs, build scripts, patches |
| [hut-os/linux-config](https://github.com/hut-os/linux-config) | Kernel defconfig and scheduler patch |
| [hut-os/docs](https://github.com/hut-os/docs) | This documentation |
| [hut-os/.github](https://github.com/hut-os/.github) | Organization profile |

---

## Quick links

```bash
git clone https://github.com/hut-os/hut-os.git
cd hut-os
# Provide a kernel bzImage (see kernel.md), then:
./build.sh initramfs
./run.sh
```

Inside HUT OS:

```text
hut@hut-os:~# about
hut@hut-os:~# hutinfo
hut@hut-os:~# hutsched on
```

---

## Project identity

- **University:** Hamedan University of Technology
- **Developer:** [Arshia Mohammadei](https://github.com/itashia)
- **Current userspace version:** HUT OS 3.0.0 (`/etc/os-release`)
- **Kernel target:** Upstream Linux (documented against 7.3-rc5) with HUTOS localversion branding

> Built from a dormitory study hall — named after the university.

## License note

Documentation in this repository is published for the HUT OS project. Kernel and BusyBox sources retain their upstream licenses (primarily GPL-2.0). See the main [hut-os](https://github.com/hut-os/hut-os) `LICENSE` file for project licensing.
