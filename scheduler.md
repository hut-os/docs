# Scheduler

## Policy vs HUTOS enhancement

| Piece | Status | Notes |
|-------|--------|-------|
| Linux fair / **EEVDF** task selection | **Implemented** (upstream) | HUT OS does **not** replace this |
| RT / deadline classes | Upstream as configured | Stock Linux |
| **HUTOS run-queue wait latency monitor** | **Implemented** | Observability only |

HUT OS does **not** ship a custom replacement scheduler. The HUTOS feature is a **diagnostics extension**.

## Feature: `CONFIG_HUTOS_SCHED_LAT`

**Name:** HUTOS Scheduler — run-queue wait latency monitor  

**Status:** **Implemented**, collection **disabled by default** (static key).

### Behavior

When enabled, the kernel records how long runnable tasks wait on the runqueue before they first run on a CPU. Measurement reuses `CONFIG_SCHED_INFO` timestamps at `sched_info_arrive()`.

Exposed metrics (per CPU and totals):

- `samples`
- `avg_ns`, `min_ns`, `max_ns`, `sum_ns`

Interface:

- `/proc/hutos_sched` — read stats; write `reset` (CAP_SYS_ADMIN) to clear
- `sysctl kernel.hutos_sched_lat` — `0` / `1`
- Kernel cmdline: `hutos_sched_lat=0|1` (also accepts `enable` / `disable`)

### Enable from HUT OS userspace

```bash
hutsched on          # or: echo 1 > /proc/sys/kernel/hutos_sched_lat
hutsched show        # cat /proc/hutos_sched
# generate load, then:
hutsched show
hutsched reset
hutsched off
```

### Design constraints (intentional)

- No change to task selection / priorities
- No hot-path `printk`
- When disabled: cost is a single `static_branch_unlikely()` check
- When enabled: lightweight per-CPU counter updates

### Source / patch

- Kernel sources (local tree): `kernel/sched/hutos_lat.c`, `hutos_lat.h`, hook in `stats.h`
- Published patch: [linux-config patches](https://github.com/hut-os/linux-config/tree/main/patches)
- Copy in main repo: [hut-os/kernel-patches](https://github.com/hut-os/hut-os/tree/main/kernel-patches)
- Distro docs: [hut-os/docs/SCHED.md](https://github.com/hut-os/hut-os/blob/main/docs/SCHED.md)

### Measured overhead (QEMU)

Workload: 1500 iterations of `/bin/busybox true` via `rdinit=/bin/hutsched-bench`.

| Mode | Uptime delta | Samples |
|------|--------------|---------|
| Monitor OFF | 8.23 s | 0 |
| Monitor ON | 7.97 s | 12745 (avg wait ≈ 170 µs) |

Overhead was within QEMU timing noise in that run.

## Experimental notes

- Enabling the monitor under heavy load increases samples; interpret latencies in the context of QEMU, not bare metal.
- `hutsched-bench` is a **test helper**, not a general-purpose system daemon.

## Planned (not implemented)

- Richer histograms / tracing integration beyond `/proc/hutos_sched`
- Automatic continuous profiling UI

See [roadmap.md](roadmap.md).
