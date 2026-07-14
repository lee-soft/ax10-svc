# ax10-svc

> ⚡ **Just want it working, not the source?** Prebuilt binaries + a one-command install are at **[archer-boot.pages.dev](https://archer-boot.pages.dev)**.

Minimal **service manager** for the TP-Link Archer AX10. This box has no `procd`, so `ax10-svc`
registers foreground daemons with **busybox-init's built-in `inittab` `respawn`** — PID-1
supervision with near-instant restart. The other `ax10-*` daemon packages depend on it.

Part of the [archer-ax10](../../../archer-ax10) project.

## Install

```sh
opkg install ax10-svc
```

## Usage

```sh
ax10-svc add <name> <foreground-command...>   # register + start (init-supervised)
ax10-svc del <name>                           # unregister + stop
ax10-svc list                                 # list ax10-managed services
```

Each service becomes a launcher script referenced by one `::respawn:` line in `/etc/inittab`,
then `kill -HUP 1` reloads init. **The command must stay in the foreground** (e.g. `uhttpd -f`,
a non-forking dropbear) or init will respawn-loop it.

Note: the stock `inittab`'s last line has no trailing newline, so `ax10-svc` rebuilds the file
(normalising it) instead of appending — otherwise the new entry would glue onto the last line.

## License

MIT (see LICENSE).
