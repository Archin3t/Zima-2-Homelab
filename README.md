# ZimaBoard 2 Homelab

> **A documented, production-style single-node Proxmox homelab built around the IceWhale ZimaBoard 2 (1664).**  
> Designed to be reproducible, hardware-agnostic, and easy to expand.

This repository documents the **architecture**, **design decisions**, and **deployment strategy** behind my personal homelab. While the reference platform is the **ZimaBoard 2 (1664)**, the same blueprint can be recreated on Intel N-series mini PCs, Intel NUCs, SFF desktops, or larger Proxmox servers with only minor hardware adjustments.

Unlike an installation guide, this repository serves as the **platform documentation** for the homelab. Individual services are documented in their own dedicated repositories.

---

# Companion Repositories

To keep documentation organized, applications are split into dedicated repositories.

| Repository | Covers |
|------------|--------|
| **[ZimaBoard-Media-Stack](https://github.com/Archin3t/Homelab-Media-Stack)** | Movies, TV, and Books — Jellyfin, Jellyseerr, Sonarr, Radarr, Prowlarr, qBittorrent, Kavita, Calibre-Web Automated, and LazyLibrarian |
| **[ZimaBoard-NAS (and Everything Else)](https://github.com/Archin3t/ZimaBoard-NAS)** | NAS, OpenMediaVault, storage passthrough, SMB/NFS shares, backups, maintenance, and additional services |

---

# HTML Documentation

📖 Browse the documentation as a website:

**https://archin3t.github.io/ZimaBoard-2-Homelab/**

The GitHub Pages version contains the same documentation in a cleaner, single-page format.

---

# Reference Hardware

| Component | Specification |
|-----------|---------------|
| **Model** | IceWhale ZimaBoard 2 (1664) |
| **CPU** | Intel N150 (4 Cores / Up to ~3.6 GHz) |
| **Memory** | 16 GB LPDDR5 |
| **Boot Drive** | 64 GB eMMC (Proxmox OS Only) |
| **Networking** | Dual 2.5 GbE |
| **Storage** | 2× SATA • 2× USB 3.0 |
| **Video Output** | Mini DisplayPort |
| **GPU** | Intel Integrated Graphics (Quick Sync / VAAPI) |

Although this documentation references the **ZimaBoard 2**, the overall architecture applies equally well to:

- Intel N100 / N150 Mini PCs
- Intel NUCs
- Small Form Factor desktops
- Custom Proxmox servers
- Multi-node Proxmox clusters

Scale storage and memory as needed while keeping workload separation intact.

---

# What It Does

```text
                 Internet
                     │
                     ▼
        Request Movie / TV Show
                     │
                     ▼
   Sonarr • Radarr • Prowlarr
                     │
                     ▼
              qBittorrent
                     │
                     ▼
               Media Storage
                     │
                     ▼
                 Jellyfin
                     │
                     ▼
          Stream Anywhere

──────────────────────────────────────

 Books / Metadata Requests
            │
            ▼
        Book Storage
            │
            ▼
 Kavita • Calibre-Web • LazyLibrarian

──────────────────────────────────────

 File Shares / Backups
            │
            ▼
 OpenMediaVault (Optional)
            │
            ▼
 SMB / NFS Shares
            │
            ▼
 PCs • Backups • Docker • Media
```

---

# Repository Documentation

| File | Purpose |
|------|---------|
| **[DOCUMENTATION.md](https://github.com/Archin3t/ZimaBoard-2-Homelab/blob/main/documentation/DOCUMENTATION.md)** | Complete platform architecture and technical documentation |
| **[CREATION-GUIDE.md](https://github.com/Archin3t/ZimaBoard-2-Homelab/blob/main/documentation/CREATION-GUIDE.md)** | Recommended build order from bare hardware to finished homelab |
| **[USER-GUIDE.md](https://github.com/Archin3t/ZimaBoard-2-Homelab/blob/main/documentation/USER-GUIDE.md)** | Day-to-day administration, maintenance, updates, and troubleshooting |
| **[SECURITY.md](https://github.com/Archin3t/ZimaBoard-2-Homelab/blob/main/documentation/SECURITY.md)** | Security practices, credential redaction, and privacy policy |
| **[index.html](https://archin3t.github.io/ZimaBoard-2-Homelab/)** | Single-page HTML documentation for GitHub Pages |

---

# Design Philosophy

This homelab follows a few simple principles:

- Infrastructure first
- Separate workloads by purpose
- Document everything
- Keep rebuilding simple
- Prefer stability over complexity
- Avoid unnecessary vendor lock-in
- Build for learning without sacrificing reliability

The goal is a platform that remains easy to understand, maintain, and rebuild years later.

---

# Security

This repository intentionally excludes sensitive information.

Examples include:

- Passwords
- API Keys
- Authentication Tokens
- VPN Credentials
- SSH Keys
- Personal IP Addresses
- Private Inventory
- Usernames

Documentation uses placeholders instead.

```text
<PROXMOX_HOST>
<JELLYFIN_HOST>
<MEDIA_STORAGE>
<USERNAME>
<PASSWORD>
```

---

# Partnership Disclosure

I am an official **IceWhale (Zima) Partner and Affiliate**.

IceWhale provides hardware for the purpose of producing educational content, tutorials, documentation, and product reviews. I may also earn a commission through qualifying purchases made using my affiliate links.

All configurations, recommendations, benchmarks, and opinions shared in this repository are based on my own real-world experience using the hardware in my personal homelab.

---

# About

This repository documents my personal Proxmox homelab as both a reference implementation and an educational resource.

While the featured hardware is the **IceWhale ZimaBoard 2**, the architecture has been designed to remain hardware-agnostic wherever possible, making it applicable to many different Proxmox systems.

---

## Credits

Created and maintained by **Archin3t**

- 🌐 GitHub: https://github.com/Archin3t
- ▶️ YouTube: https://www.youtube.com/@Archinet-Labs
- 📸 Instagram: https://www.instagram.com/Archin3t

---

© 2026 Archin3t. All Rights Reserved.

If this repository helped you build your own homelab, create documentation, make a video, or publish a guide, **please provide visible credit and link back to this repository.**
