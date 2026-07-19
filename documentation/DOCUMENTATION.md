# System Documentation — ZimaBoard 2 Homelab Blueprint

## 1. Purpose

This document describes the architecture of a **single-node Proxmox Homelab** built around the IceWhale ZimaBoard 2 (1664).

The goal is to separate workloads by responsibility so that one service, application, or storage failure does not impact the entire platform.

| Role | Responsibility |
|------|----------------|
| Hypervisor | Proxmox VE running directly on the host |
| Media player | Jellyfin streaming platform |
| Media automation | Jellyseerr + Sonarr + Radarr + Prowlarr + qBittorrent |
| Books / reading | Kavita + Calibre-Web Automated + optional LazyLibrarian |
| NAS (optional) | OpenMediaVault, network shares, backup target |

Application installation guides are maintained separately:

- **[ZimaBoard-Media-Stack](https://github.com/Archin3t/ZimaBoard-Media-Stack)** — Movies, TV, books, and media automation services
- **[ZimaBoard-NAS](https://github.com/Archin3t/ZimaBoard-NAS)** — Storage, NAS patterns, shares, backups, and additional services

---

# 2. Reference Hardware — ZimaBoard 2 1664

| Resource | Configuration |
|----------|--------------|
| Platform | IceWhale ZimaBoard 2 1664 (x86) |
| CPU | Intel N150 |
| RAM | 16 GB LPDDR5 |
| Boot Storage | 64 GB eMMC — Proxmox only |
| Guest Storage | SATA / USB / NVMe storage pool |
| Media Storage | Dedicated disk for movies, TV, and downloads |
| Books Storage | Dedicated books guest storage |
| GPU | Intel iGPU passed through for Jellyfin VAAPI / Quick Sync |

---

## Hardware Flexibility

This architecture is not locked to the ZimaBoard 2.

The same design can be adapted to:

- Intel N100/N150 mini PCs
- Intel NUC systems
- Small form factor desktops
- Custom Proxmox servers
- Larger virtualization hosts

The concepts remain the same:

- Separate infrastructure from applications
- Separate media from books
- Keep storage organized by purpose
- Scale RAM and disks based on workload

More RAM allows additional guests, more simultaneous services, and additional transcoding capacity.

---

# 3. Guest Map (Logical Template)

Guest IDs, names, hostnames, and IP addresses should remain private.

| Guest Role | Type Example | Purpose |
|------------|--------------|---------|
| Media Player | LXC or VM | Jellyfin streaming server |
| Media Automation | LXC (nesting enabled) or VM | Sonarr/Radarr/Prowlarr/qBittorrent |
| Books Platform | LXC (nesting enabled) or VM | Kavita, Calibre-Web Automated, LazyLibrarian |
| NAS | VM | OpenMediaVault, shares, backups |

---

## Example Resource Allocation

Adjust according to workload.

| Guest | vCPU | RAM | OS Disk | Additional Storage |
|-------|------|-----|---------|-------------------|
| Jellyfin | 2 | 2–4 GB | 8–16 GB | Media bind mount + GPU |
| Automation | 2 | 2–4 GB | 8–16 GB | Shared media mount |
| Books | 2 | 2–4 GB | 8–16 GB | 64–256 GB books storage |
| NAS | 1–2 | 1–2 GB | 16–32 GB | Dedicated data disk |

---

# 4. Network Design

The default design assumes a private LAN.

Recommended:

- Static IP addresses or DHCP reservations
- Internal-only services unless intentionally exposed
- Keep the real network diagram private

Never commit:

- Public IP addresses
- Domain names
- Credentials
- VPN details
- Personal inventory

---

## Service Address Template

| Service | Example |
|---------|---------|
| Proxmox | `https://<PROXMOX_HOST>:8006` |
| Jellyfin | `http://<JELLYFIN_HOST>:8096` |
| Jellyseerr | `http://<MEDIA_STACK_HOST>:5055` |
| qBittorrent | `http://<MEDIA_STACK_HOST>:8080` |
| Sonarr | `http://<MEDIA_STACK_HOST>:8989` |
| Radarr | `http://<MEDIA_STACK_HOST>:7878` |
| Prowlarr | `http://<MEDIA_STACK_HOST>:9696` |
| Kavita | `http://<BOOKS_HOST>:5000` |
| Calibre-Web Automated | `http://<BOOKS_HOST>:8083` |
| LazyLibrarian | `http://<BOOKS_HOST>:5299` |
| NAS | `http://<NAS_HOST>` |

---

# 5. Storage Philosophy

Storage is separated by purpose.

| Data | Location | Reason |
|------|----------|--------|
| Proxmox OS | eMMC / boot device | Small, isolated, easy rebuild |
| Guest disks | VM/LXC storage pool | Snapshots and backups |
| Movies / TV / Downloads | Dedicated media volume | Shared between automation and Jellyfin |
| Books / Documents | Books guest disk | Isolated from media activity |
| NAS Data | Dedicated NAS disk | User files and backups |

---

## Storage Rules

Use stable identifiers:

Recommended:

```text
/dev/disk/by-id/
```

or filesystem labels.

Avoid:

```text
/dev/sdX
```

Device names can change after reboot or hardware changes.

---

## Media Mount Requirement

The media automation stack and Jellyfin should use the **same absolute mount path**.

Example:

```text
/mnt/media
```

This allows hardlinks and efficient file management.

---

# 6. Service Pipelines

## Media Pipeline

```text
Jellyseerr
      |
      v
Sonarr / Radarr
      |
      v
Prowlarr
      |
      v
qBittorrent
      |
      v
Download Complete
      |
      v
Import + Rename + Organize
      |
      v
Jellyfin Library Update
      |
      v
Streaming
```

Flow:

1. User requests content through Jellyseerr
2. Sonarr/Radarr manages the request
3. Prowlarr provides indexer management
4. qBittorrent downloads content
5. Completed downloads are imported and organized
6. Jellyfin detects the updated library

---

## Books Pipeline

```text
Book Sources
      |
      v
Calibre-Web Automated / LazyLibrarian
      |
      v
Library Folders
      |
      v
Kavita Scan
      |
      v
Reading Platform
```

Flow:

1. Books are ingested, converted, or fetched
2. Files are organized into library folders
3. Kavita scans the library
4. Users access books through Kavita

---

# 7. Architecture Diagram

```text
                    +----------------------+
                    |   ZimaBoard Host     |
                    |      Proxmox VE      |
                    +----------+-----------+
                               |
        +----------------------+----------------------+
        |                      |                      |
        v                      v                      v

   Jellyfin              Media Stack              Books Guest
   Player                Automation               Platform

                         Jellyseerr              Kavita
                         Sonarr                  CWA
                         Radarr                  LazyLibrarian
                         Prowlarr
                         qBittorrent

        |                      |                      |
        +-----------+----------+                      |
                    |                                 |
                    v                                 v

             Media Storage                    Books Storage
             Movies / TV                      Books / Metadata

                    |
                    v

              NAS (Optional)
          Shares / Backups / Files
```

---

# 8. Related Projects

## ZimaBoard-Media-Stack

Complete guides for:

- Jellyfin
- Jellyseerr
- Sonarr
- Radarr
- Prowlarr
- qBittorrent
- Kavita
- Calibre-Web Automated
- LazyLibrarian

Repository:

https://github.com/Archin3t/ZimaBoard-Media-Stack


## ZimaBoard-NAS

Guides for:

- OpenMediaVault patterns
- Disk passthrough
- Storage planning
- Network shares
- Backups
- Additional services

Repository:

https://github.com/Archin3t/ZimaBoard-NAS

---

# 9. Out of Scope

This documentation intentionally does not cover:

- Permanent package version pinning
- Router-specific configuration steps
- Personal network layouts
- Credentials or API keys
- Private inventory
- Public exposure methods
- Reverse proxy deployment
- VPN tunnels

Document those privately for your own environment.

---

# 10. Documentation Safety

This repository contains examples only.

Never publish:

- Passwords
- API tokens
- SSH keys
- Private IP addresses
- Private domains
- Personal usernames
- Hardware inventory details

Use placeholders:

```text
<PROXMOX_HOST>
<JELLYFIN_HOST>
<MEDIA_STORAGE>
<USERNAME>
<PASSWORD>
```

---

# Credits & Attribution

Created and maintained by **Archin3t**

If this documentation, architecture, diagrams, structure, or concepts are used as a reference for:

- YouTube videos
- Blog posts
- Guides
- Courses
- Social media content
- GitHub projects
- Educational material

please provide visible credit and link back to this repository.

Substantial reuse of this documentation requires permission.

---

© 2026 Archin3t. All Rights Reserved.
