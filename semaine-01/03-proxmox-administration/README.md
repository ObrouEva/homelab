# 03 — Proxmox administration

## Objective
Get comfortable with the Proxmox interface, understand the server's baseline state, fix the recurring update error and bring the system fully up to date.

## 1. Interface tour
| Area | What I learned |
|---|---|
| Datacenter → Summary | Standalone node, 0 VMs / 0 containers, "no subscription" (free version) |
| Node `pve` → Summary | Idle baseline: CPU < 1 %, load average 0.00, ~1.6 GB of 7.55 GB RAM used, IO delay 0 % |
| Disks | NVMe, **S.M.A.R.T. PASSED**; partitions: BIOS boot / EFI / LVM |
| Network | `nic0` (physical) bridged into **`vmbr0`**, which holds the IP — future VMs plug into this virtual switch |
| System → Services | `pveproxy` (web UI on 8006), `pvedaemon` (API), `chrony` (time), `corosync` dead = normal without a cluster |
| Shell | `hostname`, `ip a` — same information as the UI, from the command line |

Knowing what "normal" looks like is the first step of monitoring and detection.

## 2. The recurring error
The task log showed `Update package database — Error: command 'apt-get update' failed: exit code 100` on every boot.

**Investigation** — the UI summary hides the cause, so I ran the command myself:
```bash
apt update
```
```
Err:4 https://enterprise.proxmox.com/debian/ceph-squid trixie InRelease
  401  Unauthorized
Err:5 https://enterprise.proxmox.com/debian/pve trixie InRelease
  401  Unauthorized
```
**Root cause:** Proxmox enables its paid *enterprise* repositories by default. Without a subscription they answer **401 Unauthorized**, and APT refuses to use a repository it can't authenticate.

## 3. Fix
In *Node → Updates → Repositories*:
1. **Disabled** `pve-enterprise` and `ceph-squid enterprise`.
2. **Added** the free `pve-no-subscription` repository.
3. Did **not** add the Ceph repository — Ceph isn't used here, and unused software is unnecessary attack surface.

```bash
apt update
# Get:5 http://download.proxmox.com/debian/pve trixie InRelease
# 212 packages can be upgraded.
```

## 4. Upgrade & reboot
```bash
apt list --upgradable | less   # review first
apt full-upgrade               # full-upgrade allows new packages such as a new kernel
uname -r                       # 7.0.2-6-pve  (old kernel still running)
# Reboot from the UI
uname -r                       # 7.0.14-28-pve (new kernel active)
```
The upgrade included many `stable-security` fixes (e.g. rsync — 33 CVEs, xz-utils, zlib, util-linux) and new versions of `pve-manager` and `qemu-server`.

## Tests
- [x] `apt update` runs with no errors
- [x] Repository status: "You get updates for Proxmox VE"
- [x] Running kernel = newly installed kernel (`7.0.14-28-pve`)

## Problems & fixes
| Problem | Fix |
|---|---|
| Status turned red ("No Proxmox VE repository enabled") after disabling enterprise | Expected intermediate state — resolved once no-subscription was added |
| "Add repository" dialog defaulted to *Enterprise* | Switched the dropdown to *No-Subscription* before confirming |

## Maintenance routine (from now on)
```bash
apt update && apt full-upgrade   # weekly
uname -r                         # reboot only if a new kernel was installed
```
No unattended upgrades on the hypervisor: if an update breaks it, every VM goes down — a human chooses the moment.

## What I learned
- **Repositories** are the "app stores" APT trusts; their signatures are what make updates safe.
- The **kernel** sits between hardware and programs. A kernel flaw compromises the whole machine, and a new kernel only takes effect after a reboot.
- **CVE** = the public ID of a known vulnerability. Updating is mostly about closing CVEs.

## Screenshots
`screenshots/` — repositories before/after, `apt update` before/after, `uname -r` before/after reboot.
