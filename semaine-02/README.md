# Week 02 — Virtual machines

**Goal:** create, size and manage virtual machines on Proxmox, and understand what a VM really costs the host.

| # | Project | Result |
|---|---|---|
| [04](04-debian-vm/) | Debian Server VM | Headless Debian 13 (2 vCPU / 2 GiB / 20 GiB LVM), DHCP behind the Wi-Fi extender, sudo-only admin, Guest Agent working |
| 05 | Second Linux VM | ⏳ |
| [06](06-snapshots/) | Snapshots | Took a snapshot, made the VM unbootable (`rm -rf /boot/*`), rolled back in one click, measured copy-on-write with `lvs` |

## Skills practised
vCPU / vRAM / virtual disks · VirtIO · thin provisioning & discard · Linux bridge · DHCP · Debian installer · LVM (PV / VG / LV) · sudo vs root · QEMU Guest Agent · boot order & pending changes · snapshots & copy-on-write · rollback · relative vs absolute paths

## Key lessons
1. **Measure, don't assume.** `free -h` before and after showed the VM really costs ~0.8 GiB on the host, including QEMU overhead.
2. **Read every summary screen before confirming.** That's how I caught the wrong volume group name.
3. **Space toggles, Enter validates.** In text installers, mixing them up installs things you didn't choose.
4. **A snapshot is an undo button, not a backup.** It lives on the same SSD and should be deleted once the change is confirmed.
