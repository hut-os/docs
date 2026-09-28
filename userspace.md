# Userspace

## Overview

HUT OS userspace is deliberately small:

| Component | Role | Status |
|-----------|------|--------|
| BusyBox (static) | Core utilities + applets | **Implemented** |
| Custom `/init` | Boot bring-up | **Implemented** |
| Zsh | Interactive shell | **Implemented** |
| Oh My Zsh (minimal) | Shell framework files | **Implemented** |
| Account files | `passwd` / `group` / `shadow` | **Implemented** |

There is **no** systemd, OpenRC, or package manager in the current tree.

## BusyBox

- Binary: `rootfs/bin/busybox` (statically linked)
- Many commands are symlinks to BusyBox (`ls`, `mount`, `ps`, `ifconfig`, `mdev`, …)

Useful examples:

```bash
ps
dmesg | head
mount
free
uname -a
```

## Interactive shell: Zsh + Oh My Zsh

- Default root shell: `/bin/zsh` (`/etc/passwd`)
- Config: `/root/.zshrc`
- Oh My Zsh tree: `/root/.oh-my-zsh` (trimmed install)
- Prompt style: `hut@hut-os:~#` (colored)

If Zsh is missing, `/init` falls back to BusyBox `ash`.

Oh My Zsh auto-update is disabled in configuration (no network dependency at shell start for updates).

Host helper scripts in the main repo (for refreshing Zsh into `rootfs/`):

- `setup-zsh.sh`
- `fix-zsh-modules.sh`

Status: **Implemented** for the packaged rootfs.

## Accounts

| User | UID | Shell | Home |
|------|-----|-------|------|
| `root` | 0 | `/bin/zsh` | `/root` |
| `hut` | 1000 | `/bin/zsh` | `/home/hut` |
| `nobody` | 65534 | `/bin/false` | `/nonexistent` |

Password database exists (`/etc/shadow`). Interactive login via `getty`/`login` is **not** the primary demo path; QEMU boots directly into a root Zsh via `/init`.

## Hostname / OS identity

- Hostname: `hut-os` (`/etc/hostname`)
- `/etc/os-release` — HUT OS 3.0.0 metadata
- `/etc/hutos-version` — `3.0.0`

## Init behavior (summary)

`/init` (BusyBox ash):

1. Mounts virtual filesystems
2. Runs `mdev -s` with a timeout (avoids hangs on CD-ROM under ISO boot)
3. Sets hostname
4. Brings up `lo`; attempts `eth0` + DHCP
5. Prints banner and welcome text
6. Starts Zsh in a restart loop (exiting the shell does not kill PID 1)

Details: [boot.md](boot.md).

## See also

- [Root filesystem](rootfs.md)
- [System tools](system-tools.md)
- [Networking](networking.md)
