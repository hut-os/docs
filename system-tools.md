# System tools

HUT OS adds a small set of project-specific commands on top of BusyBox.

## HUT OS commands (**Implemented**)

### `about`

Prints the HUT OS identity page: university branding, developer information, and the dormitory origin story.

```bash
about
```

### `hutinfo`

Compact runtime summary: OS version, hostname, kernel, architecture, CPU string (from `/proc/cpuinfo` when available), memory, load, uptime, root filesystem usage.

```bash
hutinfo
```

### `hutsched`

Helper for the HUTOS scheduler latency monitor (requires a kernel built with `CONFIG_HUTOS_SCHED_LAT`).

```bash
hutsched show      # default
hutsched on
hutsched off
hutsched reset
hutsched help
```

Underlying interfaces: `/proc/hutos_sched`, `/proc/sys/kernel/hutos_sched_lat`.

### `hutsched-bench`

**Testing helper.** Intended to be run as `rdinit=/bin/hutsched-bench` under QEMU to compare monitor off vs on. Not part of normal interactive boot.

See [testing.md](testing.md) and [scheduler.md](scheduler.md).

## BusyBox utilities commonly used in demos

These are **Implemented** as BusyBox applets/symlinks in the current rootfs (non-exhaustive):

```text
ls, cat, cp, mv, rm, mkdir, mount, umount, ps, kill, dmesg, uname,
free, uptime, hostname, ifconfig, ip, route, ping, udhcpc, mdev,
chmod, chown, id, whoami, clear, vi, top, tar, gzip, wget, sysctl, …
```

```bash
ps
dmesg | tail
mount
free
uname -a
sysctl kernel.hutos_sched_lat
```

## Shell aliases (Zsh)

Configured in `/root/.zshrc` (examples):

- `about`, `hutinfo`
- `ll`, `la`, `l`, `cls`
- `huname`

## Planned tools

No additional HUTOS-branded daemons are documented as shipped. Future ideas belong in [roadmap.md](roadmap.md).

## See also

- [Userspace](userspace.md)
- [Scheduler](scheduler.md)
