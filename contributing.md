# Contributing

Thank you for contributing to HUT OS.

## Repositories

| Change type | Where to PR |
|-------------|-------------|
| Rootfs, scripts, distro docs under `hut-os/` | [hut-os/hut-os](https://github.com/hut-os/hut-os) |
| Kernel defconfig / patches | [hut-os/linux-config](https://github.com/hut-os/linux-config) |
| Official Markdown manuals | [hut-os/docs](https://github.com/hut-os/docs) (this repo) |

## Workflow

1. Fork or push a branch with write access.
2. Branch from `main` with a descriptive name (`feature/…`, `docs/…`, `fix/…`).
3. Make focused commits ([Conventional Commits](https://www.conventionalcommits.org/) preferred).
4. Open a Pull Request against `main`.
5. Keep PRs reviewable; avoid unrelated reformatting.

Examples:

```text
docs: clarify QEMU disk boot cmdline
feat: add hutsched reset confirmation
sched: tighten HUTOS latency sysctl permissions
```

## Documentation contributions (this repo)

- Write in **English**
- Prefer accuracy over marketing language
- Mark features as **Implemented**, **Experimental**, or **Planned**
- Do not document APIs/commands that do not exist
- Include practical commands when helpful

## Code of conduct

Be respectful. HUT OS is a student-led educational operating-system project associated with Hamedan University of Technology.

## License

Respect upstream licenses for Linux, BusyBox, Zsh, and Oh My Zsh. Project licensing for the main tree is described in [hut-os/LICENSE](https://github.com/hut-os/hut-os/blob/main/LICENSE).
