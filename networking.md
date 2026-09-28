# Networking

Status: **Implemented** for QEMU user networking demos. Not a full network stack management framework.

## What `/init` does

1. Brings up **loopback** (`ifconfig lo 127.0.0.1 up` or `ip link set lo up`).
2. If `/sys/class/net/eth0` exists:
   - Brings the interface up
   - Runs `udhcpc -i eth0 -n -q -t 3 -T 1` (best-effort; failures do not stop boot)

## QEMU setup used by scripts

```text
-nic user,model=e1000
```

This provides a virtual NIC visible as `eth0` with DHCP from QEMU’s user-mode network (typically gateway `10.0.2.2`, guest DNS often `10.0.2.3`).

`/etc/resolv.conf` ships with nameserver entries suitable for that environment.

## Configuration files

| File | Role |
|------|------|
| `/etc/network/interfaces` | Declares `lo` (loopback) and `eth0` (dhcp) for BusyBox ifup/ifdown |
| `/etc/hosts` | `localhost`, `hut-os` |
| `/etc/resolv.conf` | DNS stubs for QEMU user net |

## Useful commands inside HUT OS

```bash
ifconfig
ip addr
ip route
ping -c 2 10.0.2.2     # QEMU gateway (if usernet is up)
cat /etc/resolv.conf
```

## What is **not** claimed

- No firewall / nftables management UI
- No NetworkManager
- No guaranteed outbound Internet from every host setup
- No Wi-Fi stack

## Experimental / optional

- Using BusyBox `ifup -a` explicitly (interfaces file exists; `/init` currently brings links up directly)
- Bridged or tap networking — possible with custom QEMU flags, **not** packaged as the default workflow

## See also

- [Boot](boot.md)
- [System tools](system-tools.md)
