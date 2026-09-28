# Testing

## Minimum smoke test (**Implemented**)

```bash
./build.sh initramfs
./run.sh
```

Verify:

1. Boot reaches `hut@hut-os:~#`
2. Banner / welcome text appears
3. `about` and `hutinfo` run
4. Basic BusyBox commands work (`ls`, `uname`, `ps`)

## Disk / ISO (**Implemented**)

```bash
./scripts/run-disk.sh
./scripts/run-iso.sh
```

Confirm each path reaches Zsh. ISO boots through GRUB first.

## Scheduler feature tests

Requires a kernel built with `CONFIG_HUTOS_SCHED_LAT=y` and the HUTOS patch applied.

### Interactive

```bash
hutsched on
# create load (commands, loops, etc.)
hutsched show
hutsched off
```

### Automated helpers in tree

```bash
./scripts/test-hutos-sched.sh
```

Microbenchmark init:

```bash
qemu-system-x86_64 -m 256 \
  -kernel kernel/arch/x86/boot/bzImage \
  -initrd build/initramfs.cpio.gz \
  -append "console=ttyS0 rdinit=/bin/hutsched-bench" \
  -nographic
```

Look for `BENCH_OFF_*` / `BENCH_ON_*` lines and `/proc/hutos_sched` totals.

## Networking smoke

With default `-nic user,model=e1000`:

```bash
ifconfig eth0
# optional:
ping -c 2 10.0.2.2
```

DHCP may fail in some environments; boot must still succeed.

## CI

The main repository includes a GitHub Actions workflow that checks required paths and BusyBox presence. It does **not** currently boot QEMU in CI.

## Reporting bugs

Include:

- Kernel version (`uname -a`)
- Boot path (initramfs / disk / ISO)
- Host OS and QEMU version
- Relevant `dmesg` / serial log excerpts

## See also

- [Scheduler](scheduler.md)
- [Boot](boot.md)
- [Contributing](contributing.md)
