# Creation Guide — ZimaBoard 2 Homelab Blueprint

This guide covers building the **Proxmox foundation**, creating guests, preparing storage, and connecting the platform together.

Application installation steps are intentionally kept separate and are maintained in the companion repositories.

---

# 0. Before You Start

## Hardware Checklist

- [ ] IceWhale ZimaBoard 2 (or compatible x86 system) ready
- [ ] Display access or SSH access available
- [ ] Proxmox VE installer prepared
- [ ] Dedicated media storage disk available
- [ ] Guest storage pool available
- [ ] Network connection available

## Planning Checklist

Before installation, prepare a private notes file.

Keep the following private:

- IP addresses
- Hostnames
- VM/LXC IDs
- Passwords
- API keys
- Domain names
- Storage inventory

Do **not** store private infrastructure details inside Git.

---

# 1. Install Proxmox VE

Install Proxmox VE as the foundation of the platform.

## Recommended Layout

| Storage | Purpose |
|---------|---------|
| eMMC / Boot SSD | Proxmox operating system only |
| Larger storage pool | VM/LXC disks, snapshots, backups |
| Dedicated media disk | Movies, TV, downloads |
| Dedicated books storage | Books and reading library |

---

## Installation Steps

1. Install Proxmox VE to the boot device.

2. Configure:

   - Strong root password
   - Network settings
   - Correct hostname
   - Time zone

3. Update the Proxmox node.

4. Create or configure guest storage.

5. Confirm the web interface:

```text
https://<PROXMOX_HOST>:8006
```

---

# 2. Prepare Media Storage

The media volume stores:

- Movies
- TV shows
- Downloads
- Completed imports

It is shared between:

- Media automation services
- Jellyfin

---

## Storage Preparation

1. Create the filesystem of your choice.

2. Assign a stable filesystem label.

Example:

```text
media-library
```

3. Mount the filesystem on the Proxmox host.

Example:

```text
/mnt/media
```

4. Configure persistent mounting.

Preferred methods:

```text
LABEL=<MEDIA_LABEL>
```

or:

```text
/dev/disk/by-id/
```

Avoid:

```text
/dev/sdX
```

Device letters can change after reboot or hardware changes.

---

# 3. Create Guests

The following layout is a recommended starting point.

Adjust resources depending on your hardware and workload.

---

# Media Player — Jellyfin

Purpose:

- Streaming platform
- Hardware accelerated playback
- Media library access

| Setting | Example |
|---------|---------|
| Type | LXC (Debian) or VM |
| vCPU | 2 |
| RAM | 2–4 GB |
| OS Disk | 8–16 GB |
| Storage | Media bind mount |
| GPU | Optional `/dev/dri` passthrough |
| Startup | Enable auto-start |

---

## Media Automation — Jellyseerr + *arr + qBittorrent

Purpose:

- Requests
- Library management
- Downloads
- Import automation

Services:

- Jellyseerr
- Sonarr
- Radarr
- Prowlarr
- qBittorrent

| Setting | Example |
|---------|---------|
| Type | LXC with nesting or VM |
| vCPU | 2 |
| RAM | 2–4 GB |
| OS Disk | 8–16 GB |
| Storage | Same media mount path as Jellyfin |

Important:

The automation guest and Jellyfin guest should use the same absolute path.

Example:

```text
/mnt/media
```

This allows efficient hardlink-based workflows.

---

# Books Platform — Kavita + Calibre-Web Automated + Optional LazyLibrarian

Purpose:

- Book management
- Reading platform
- Metadata organization

| Setting | Example |
|---------|---------|
| Type | LXC with nesting or VM |
| vCPU | 2 |
| RAM | 2–4 GB |
| OS Disk | 8–16 GB |
| Storage | Dedicated books storage |

Important:

The books guest should remain separate from the movie/TV media volume.

Recommended:

```text
Books Storage
      |
      +-- Library
      +-- Metadata
      +-- Imports
```

---

# NAS (Optional)

Purpose:

- Network shares
- Backup destination
- File storage

Common deployment:

| Setting | Example |
|---------|---------|
| Type | VM |
| Platform | OpenMediaVault |
| Storage | Passthrough disk or dedicated data disk |

See:

**[ZimaBoard-NAS](https://github.com/Archin3t/ZimaBoard-NAS)**

for storage and NAS-specific documentation.

---

# 4. Networking

The recommended design uses a standard LAN bridge.

Example:

```text
Internet
   |
Router
   |
LAN
   |
vmbr0
   |
+---------+---------+---------+
|         |         |         |
Jellyfin  Media     Books     NAS
Guest     Guest     Guest     Guest
```

---

## Network Checklist

Verify:

- [ ] Guests receive network access
- [ ] Jellyfin can access media storage
- [ ] Automation can access downloads
- [ ] Automation can communicate with Jellyfin
- [ ] Web interfaces load correctly

Use:

- Static IP addresses
- DHCP reservations

Choose whichever works best for your network.

---

# 5. Install Applications

After the platform is created, continue with the companion repositories.

---

## Media Stack

Repository:

**[ZimaBoard-Media-Stack](https://github.com/Archin3t/ZimaBoard-Media-Stack)**

Covers:

- Jellyfin
- Jellyseerr
- Sonarr
- Radarr
- Prowlarr
- qBittorrent
- Kavita
- Calibre-Web Automated
- LazyLibrarian

---

## NAS Platform

Repository:

**[ZimaBoard-NAS](https://github.com/Archin3t/ZimaBoard-NAS)**

Covers:

- OpenMediaVault
- Storage layouts
- Disk passthrough
- Shares
- Backups
- Additional services

---

# 6. Backup Strategy

A homelab is only as reliable as its recovery plan.

| Data | Recommended Backup |
|------|--------------------|
| Guest disks | Proxmox backup jobs |
| VM/LXC configuration | Proxmox backups |
| Media library | Separate backup/sync strategy |
| Books library | Backup or sync library storage |
| Secrets | Password manager export |

---

## Important Backup Rules

Never store:

- Passwords
- API keys
- Tokens
- SSH keys

inside Git repositories.

Use:

- Password managers
- Encrypted backups
- Private storage

---

# 7. Final Validation Checklist

Before considering the platform complete:

- [ ] Proxmox accessible
- [ ] Storage mounts survive reboot
- [ ] Guests start automatically
- [ ] Jellyfin detects media
- [ ] Automation imports correctly
- [ ] Books library scans correctly
- [ ] Backups tested
- [ ] Credentials stored securely

---

# Credits & Attribution

Created and maintained by **Archin3t**

This guide represents original documentation, architecture planning, and deployment methodology created for the **ZimaBoard 2 Homelab Blueprint**.

If this guide, structure, diagrams, or documentation are referenced for:

- Videos
- Articles
- Blog posts
- Courses
- GitHub projects
- Social media content
- Educational material

please provide visible credit and a link back to this repository.

Substantial reproduction of this documentation requires permission.

---

© 2026 Archin3t. All Rights Reserved.
