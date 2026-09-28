# Building

Status: **Implemented** unified build entrypoint.

## Command

```bash
./build.sh [all|initramfs|disk|iso]
```

| Target | Output |
|--------|--------|
| `initramfs` (default piece of `all`) | `build/initramfs.cpio.gz` |
| `disk` | `build/hutos.img` (ext4, label `HUTOS`) |
| `iso` | `build/hutos.iso` (GRUB + kernel + initrd) |
| `all` | all of the above |

Also updates convenience copies/symlinks such as `initramfs.cpio.gz` / `hutos.img` / `hutos.iso` at the repo root when scripts do so.

## Pipeline

```text
rootfs/  ──pack──►  initramfs.cpio.gz
   │
   ├──stage+mkfs.ext4 -d──►  hutos.img  (+ /boot/bzImage)
   │
initramfs + bzImage ──grub-mkrescue──►  hutos.iso
```

Disk images are created **without sudo** using `mkfs.ext4 -d <staging-dir>`.

## Requirements by target

| Target | Needs |
|--------|-------|
| initramfs | `cpio`, `gzip`, populated `rootfs/` |
| disk | `mkfs.ext4`, kernel `bzImage` |
| iso | `grub-mkrescue`, `xorriso`, kernel + initramfs |

## Environment knobs

- `HUTOS_DISK_MB` — disk image size in MiB (default `256` in `scripts/build-disk.sh`)

## What build does **not** do

- It does **not** compile the Linux kernel.
- It does **not** download packages from the network at build time (Zsh/Oh My Zsh are expected to already be present under `rootfs/` for a full interactive experience).

## Clean rebuild tips

```bash
./build.sh initramfs
ls -lh build/
```

After changing only docs, no rebuild is required for QEMU unless you also change `rootfs/`.

## See also

- [Getting started](getting-started.md)
- [Boot](boot.md)
- [Kernel](kernel.md)
