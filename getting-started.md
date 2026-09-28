# Getting started

Status of this guide: **Implemented** workflow for developing and booting HUT OS under QEMU.

## Prerequisites (host)

| Tool | Purpose |
|------|---------|
| `git` | Clone repositories |
| `qemu-system-x86_64` | Emulation |
| `cpio`, `gzip` | Initramfs packaging |
| `mkfs.ext4` | Persistent disk image (`./build.sh disk`) |
| `grub-mkrescue`, `xorriso` | GRUB ISO (`./build.sh iso`) — optional |
| GCC / make | Building upstream Linux (kernel) |

On Ubuntu/Debian-style hosts:

```bash
sudo apt-get install -y qemu-system-x86 cpio gzip e2fsprogs \
  grub-pc-bin grub-common xorriso mtools build-essential flex bison libncurses-dev bc
```

## Clone

```bash
git clone https://github.com/hut-os/hut-os.git
cd hut-os
```

## Provide a kernel image

HUT OS does **not** vendor the full Linux source tree in `hut-os/hut-os`.

You need `kernel/arch/x86/boot/bzImage`. Typical approach:

1. Clone upstream Linux and check out a tested tag (e.g. `v7.3-rc5`).
2. Apply the HUTOS defconfig (and scheduler patch if desired) from [linux-config](https://github.com/hut-os/linux-config).
3. Build `bzImage` and place it where the scripts expect:

```bash
mkdir -p kernel/arch/x86/boot
cp /path/to/linux/arch/x86/boot/bzImage kernel/arch/x86/boot/bzImage
```

Details: [kernel.md](kernel.md).

## First build and boot

```bash
./build.sh initramfs   # → build/initramfs.cpio.gz
./run.sh               # QEMU, initramfs path
```

Exit QEMU: **Ctrl+A**, then **X**.

## First commands inside HUT OS

```text
hut@hut-os:~# about      # project story / identity
hut@hut-os:~# hutinfo    # version, kernel, memory, uptime
hut@hut-os:~# hutsched on
hut@hut-os:~# hutsched show
hut@hut-os:~# ifconfig
hut@hut-os:~# uname -a
```

## Other boot modes (**Implemented**)

```bash
./build.sh disk
./scripts/run-disk.sh    # ext4 root on virtio (/dev/vda)

./build.sh iso
./scripts/run-iso.sh     # GRUB ISO → kernel + initramfs
```

## Read next

- [Architecture](architecture.md)
- [Building](building.md)
- [Boot](boot.md)
