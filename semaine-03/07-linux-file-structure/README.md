# 07 — Linux file structure

## Objective
Understand **where things live** on a Linux server (the FHS, Filesystem Hierarchy Standard), then:
1. explore the real tree on `debian-01`,
2. create a clean directory layout for several services, placed where the standard expects them,
3. write my own **command guide** ([`cheatsheet.md`](../../cheatsheet.md)), explained in my own words.

## Concept: one single tree
Linux has **one tree** starting at `/`. There are no drive letters: a disk or partition is **mounted** onto a folder (like `ubuntu-lv` mounted on `/` in project 05). The layout follows the **FHS**, so every Linux server is organised the same way and an admin knows where to look on any machine.

| Directory | What lives there | Already met in this lab |
|---|---|---|
| `/etc` | Configuration files (text) | `/etc/hosts`, `/etc/ssh/` |
| `/var` | Variable data: logs (`/var/log`), caches, databases, web content | — |
| `/srv` | Data a server **serves** (website, files) | — |
| `/opt` | Add-on software installed outside the package manager | — |
| `/usr` | Installed programs and libraries (`/usr/bin`, `/usr/lib`) | `/usr/share/zoneinfo/Europe/Paris` |
| `/home`, `/root` | Users' personal files / the root user's home | `~` = `/home/eva` |
| `/boot` | Kernel + bootloader | deleted on purpose in project 06 → `grub rescue>` |
| `/dev`, `/proc`, `/tmp` | Devices as files, live kernel/process info, temporary files | `/dev/urandom`, `/dev/mapper/…` |

**Key idea:** a service is **split by type of file**, not kept in one folder: program in `/usr/bin`, config in `/etc/<service>`, logs in `/var/log/<service>`, data in `/var/lib/<service>` or `/srv`.

## Prediction before starting
_(to fill in)_
