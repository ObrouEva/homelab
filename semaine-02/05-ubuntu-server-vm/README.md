# 05 — Second Linux VM: Ubuntu Server (and how it differs from Debian)

> 🚧 **In progress.** This README is written step by step during the project, mistakes included.

## Objective
Install **Ubuntu Server 26.04.1 LTS** as a second VM next to `debian-01`, then **compare the two distributions**: installer, defaults, resource usage and philosophy. The goal isn't just "I installed Ubuntu", but understanding why a company would choose one or the other.

## Concept: same family, different choices
Ubuntu is built **on top of Debian**: Canonical takes Debian, adds its own choices and tooling, and ships a release every 6 months, with an LTS every 2 years. Same family (`apt`, `systemd`, same filesystem layout), but different defaults.

| | Debian 13 | Ubuntu Server 26.04 LTS |
|---|---|---|
| Made by | Volunteer community | A company (Canonical) |
| Philosophy | Stable, minimal, "you decide" | Up to date, "batteries included" |
| Installer | Classic text installer (d-i) | **Subiquity**, a newer, guided installer |
| Root account | Optional (I disabled it) | Always disabled, sudo user by default |
| Extras | Almost none | Snaps, cloud-init, Ubuntu Pro… |
| Where you meet it | Many servers and appliances (**Proxmox itself runs on Debian**) | The most common Linux in the public cloud (AWS / Azure / GCP images) |

Why both matter for my goal: Debian is under my hypervisor; Ubuntu is what I'll meet in the cloud (Phase 4).

## Prediction before starting
> The Ubuntu Server ISO is **2.7 GB**, the Debian netinst was **756 MB**. Why ~4× bigger?

_My answer and the real reason will be added here._

## Steps
_Added as I go._

## Problems & fixes
_Added as I go._
