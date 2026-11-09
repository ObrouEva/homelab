# Week 01 — Know your server & Proxmox

**Goal:** turn a refurbished mini-PC into a working, up-to-date hypervisor that I administer remotely from my Mac.

| # | Project | Result |
|---|---|---|
| [01](01-hardware-inventory/) | Hardware inventory | Exact specs confirmed before wiping Windows; upgrade path planned |
| [02](02-proxmox-install/) | Proxmox VE install | BIOS prepared, ISO verified, Proxmox 9.2 installed with a static IP |
| [03](03-proxmox-administration/) | Proxmox administration | Web UI explored, update repositories fixed, 212 packages upgraded, new kernel booted |
| [03b](03b-server-relocation/) | Server relocation | Moved behind a Wi-Fi extender (layer-2 bridge): same static IP, verified with ping/TTL |

## Skills practised
BIOS/UEFI · hardware virtualisation (VT-x / VT-d) · type-1 hypervisor · ISO integrity (SHA256) · static IP addressing · Linux package management (APT) · reading logs and error codes · kernel updates · layer-2 bridging vs routing · TTL analysis · safe shutdown & power hygiene

## Key lessons
1. **Always verify what you download.** My first ISO download failed its SHA256 check — I re-downloaded instead of installing a file I couldn't trust.
2. **Read the real error, not the summary.** The UI only said `exit code 100`; running `apt update` in the shell showed the actual cause: `401 Unauthorized`.
3. **A kernel update isn't applied until you reboot.** `uname -r` before and after proved it.
