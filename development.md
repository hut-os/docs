# Development

## Organization layout

| Repository | Contents |
|------------|----------|
| [hut-os/hut-os](https://github.com/hut-os/hut-os) | `rootfs/`, `scripts/`, `configs/`, `kernel-patches/`, build/run entrypoints |
| [hut-os/linux-config](https://github.com/hut-os/linux-config) | `hutos_defconfig`, kernel patches |
| [hut-os/docs](https://github.com/hut-os/docs) | This documentation |
| Local `kernel/` directory | Optional full Linux checkout + build outputs (often gitignored in the distro repo) |

## Recommended local layout

```text
HUTOS/                    # clone of hut-os/hut-os
├── rootfs/
├── scripts/
├── configs/hutos_defconfig
├── kernel-patches/
├── build.sh
├── run.sh
└── kernel/               # local Linux tree or symlink (not always in git)
    └── arch/x86/boot/bzImage
```

## Typical edit cycle

1. Change files under `rootfs/` or `scripts/`.
2. `./build.sh initramfs`
3. `./run.sh` and verify.
4. For kernel work: patch/build Linux, copy `bzImage`, reboot under QEMU.
5. Open a feature branch and PR against `main` (see [contributing.md](contributing.md)).

## Coding notes

- `/init` and helpers must remain **BusyBox ash**-compatible (no Bash-only syntax).
- Prefer absolute paths in early boot where helpful.
- Keep cosmetic boot features from breaking the path to a shell.
- Do not introduce network requirements into boot or shell startup.

## Debugging tips

```bash
# Host: inspect packaged initramfs
./inspect-initramfs.sh   # if present in tree

# Guest:
dmesg | grep HUTOS
cat /proc/hutos_sched
cat /proc/cmdline
```

Serial console logs are the primary debug surface under `-nographic`.

## See also

- [Building](building.md)
- [Testing](testing.md)
- [Contributing](contributing.md)
