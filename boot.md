# Boot

## Boot paths (**Implemented**)

HUT OS supports three QEMU-oriented boot paths:

### 1. Initramfs (fastest development)

```bash
./run.sh
# equivalent core:
# qemu-system-x86_64 -kernel bzImage -initrd initramfs.cpio.gz \
#   -append "console=ttyS0" -nic user,model=e1000 -nographic
```

Flow:

```text
QEMU → bzImage → unpack initramfs → /init → Zsh
```

### 2. Persistent ext4 disk

```bash
./build.sh disk
./scripts/run-disk.sh
```

Flow:

```text
QEMU → bzImage + virtio disk → root=/dev/vda → /init on ext4 → Zsh
```

Kernel cmdline (script): `root=/dev/vda rw rootwait console=ttyS0 init=/init`

### 3. GRUB ISO

```bash
./build.sh iso
./scripts/run-iso.sh
```

Flow:

```text
QEMU CD-ROM → GRUB → linux + initrd → /init → Zsh
```

GRUB config places `console=ttyS0` last so serial output works with `-nographic`.

## `/init` stages (**Implemented**)

1. Clear screen; print boot header  
2. Mount `proc`, `sysfs`, `devtmpfs` (fallback tmpfs), `devpts`, `tmpfs` for `/tmp` and `/run`  
3. Configure hotplug helper; run `mdev -s` with timeout  
4. Set hostname from `/etc/hostname`  
5. Bring up loopback; if `eth0` exists, up + best-effort DHCP  
6. Display `/etc/hut-banner.txt` and welcome / developer text  
7. `cd /root`; start `/bin/zsh` in a loop  

## Optional cmdline flags

| Flag | Effect |
|------|--------|
| `console=ttyS0` | Serial console for QEMU `-nographic` |
| `hutos_sched_lat=1` | Enable HUTOS scheduler latency collection at boot |
| `root=/dev/vda rootwait` | Disk boot (see `run-disk.sh`) |
| `rdinit=/bin/hutsched-bench` | Run scheduler bench instead of normal init (**testing**) |

## Exit / shutdown

- Leave QEMU: **Ctrl+A**, then **X**
- From inside (BusyBox): `poweroff` / `reboot` applets exist; behavior depends on kernel + how QEMU is quit

## See also

- [Building](building.md)
- [Networking](networking.md)
- [Testing](testing.md)
