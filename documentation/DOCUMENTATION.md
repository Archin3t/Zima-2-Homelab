
---

### 2 — `documentation/DOCUMENTATION.md`

```markdown
# System Documentation — ZimaBoard 2 Homelab Blueprint

## 1. Purpose

A single-node Homelab that separates concerns so one failure/domain doesn’t own everything:

| Role | Responsibility |
|------|----------------|
| Hypervisor | Proxmox VE on the host |
| Media player | Jellyfin (stream movies/TV) |
| Media automation | Jellyseerr + Sonarr/Radarr + Prowlarr + qBittorrent |
| Books / reading | Kavita + Calibre-Web Automated (+ optional LazyLibrarian) on **separate disk** |
| NAS (optional) | OpenMediaVault / shares / backup target |

App install details: **[ZimaBoard-Media-Stack](https://github.com/Archin3t/ZimaBoard-Media-Stack)** and **[ZimaBoard-NAS-Platform](https://github.com/Archin3t/ZimaBoard-NAS-Platform)**.

## 2. Reference hardware — ZimaBoard 2 1664

| Resource | This build |
|----------|------------|
| Platform | IceWhale ZimaBoard 2 1664 (x86) |
| CPU | Intel N150 |
| RAM | 16 GB |
| Boot disk | 64 GB eMMC — Proxmox only |
| Guest disk pool | Larger SATA/USB/NVMe pool for VM/LXC disks |
| Media volume | Large dedicated disk/USB for movies/TV/downloads |
| Books volume | Space **inside** the books guest disk (not on media volume) |
| GPU | Intel iGPU → optional `/dev/dri` to Jellyfin for VAAPI |

**Portable ideas:** any low-power x86 with 16 GB RAM can follow this layout. More RAM = happier concurrent transcodes + more guests.

## 3. Guest map (logical template)

Assign your own IDs, names, and addresses privately.

| Guest role | Type (example) | Notes |
|------------|----------------|-------|
| Media player | LXC or VM | Bind-mount media volume; optional GPU |
| Media automation | LXC (nesting) or VM | **Same** media mount path as player (hardlinks) |
| Books | LXC (nesting) or VM | Own large disk; do not mount media library |
| NAS | VM (common) | Passthrough or virtio data disk |

Example starting resources (tune to taste):

| Guest | vCPU | RAM | OS disk | Extra |
|-------|------|-----|---------|--------|
| Player | 2 | 2–4 GB | 8–16 GB | media bind + `/dev/dri` |
| Automation | 2 | 2–4 GB | 8–16 GB | media bind |
| Books | 2 | 2–4 GB | **64–256 GB** | Docker/compose OK |
| NAS | 1–2 | 1–2 GB | 16–32 GB | data disk passthrough |

## 4. Network (generic)

- Private LAN unless you deliberately add reverse proxy + TLS  
- Static IP or DHCP reservation per guest  
- Keep the real map in a password manager / private wiki — **not** public git  

| Role | Placeholder |
|------|-------------|
| Hypervisor | `https://<PROXMOX_HOST>:8006` |
| Player | `http://<JELLYFIN_HOST>:8096` |
| Request UI | `http://<MEDIA_STACK_HOST>:5055` |
| Download client | `http://<MEDIA_STACK_HOST>:8080` |
| *arr | Sonarr `:8989` · Radarr `:7878` · Prowlarr `:9696` |
| Books reader | `http://<BOOKS_HOST>:5000` |
| Book tools | CWA `:8083` · LazyLibrarian `:5299` |
| NAS | `http://<NAS_HOST>` |

## 5. Storage philosophy

| Data | Where | Why |
|------|-------|-----|
| Hypervisor OS | eMMC / boot device | Small, rebuildable |
| Guest virtual disks | Guest storage pool | Snapshots/backups |
| Movies / TV / torrent completes | Dedicated media volume | Shared by player + automation |
| Books / textbooks | Books guest disk | Isolated from media churn |
| NAS shares | NAS data disk | User files / backups |

**Mount by LABEL or `/dev/disk/by-id/...` — never `/dev/sdX`.**

Bind-mount media into player + automation at the **same absolute path** (example: `/mnt/media`) so hardlinks work.

## 6. How stacks connect

```text
                    +------------------+
                    |  ZimaBoard host  |
                    |    (Proxmox)     |
                    +--------+---------+
                             |
     +-----------+-----------+-----------+-----------+
     |           |           |           |           |
     v           v           v           v           v
 Jellyfin    Media stack   Books      NAS (opt)   (future)
 (player)    (*arr/qBit)   (Kavita)   (OMV)
     ^           |           |
     |    shared media disk  | own disk
     +-----------+           v
                 |      book library
                 v
           media files
