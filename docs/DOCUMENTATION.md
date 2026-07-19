# System Documentation — Homelab Blueprint

## 1. Purpose

A single-node Homelab that separates concerns:

| Role | Responsibility |
|---|---|
| Hypervisor | Proxmox VE on the host |
| Media player | Jellyfin (or similar) |
| Media automation | Request UI + Sonarr/Radarr + indexers + torrent client |
| NAS (optional) | File shares / backup target (e.g. OpenMediaVault) |
| Books / reading | Separate guest with its own disk (Kavita + Calibre stack) |

## 2. Hardware profile (example)

Values below are **examples**. Size to your board.

| Resource | Example |
|---|---|
| Platform | Low-power x86 SBC / mini PC (e.g. Intel N-series) |
| RAM | 16 GB class |
| Boot disk | Small internal eMMC/SSD for hypervisor OS only |
| Guest disk pool | Larger HDD/SSD for VM/LXC disks |
| Media volume | Large external/internal disk dedicated to movies/TV |
| Books volume | Space **inside** the books guest disk (not on media volume) |
| GPU | Integrated Intel graphics for VAAPI transcoding (optional) |

## 3. Guest map (logical)

Assign your own IDs, names, and addresses.

| Guest role | Type (example) | Notes |
|---|---|---|
| Media player | LXC or VM | Bind-mount media volume; optional GPU passthrough/`/dev/dri` |
| Media automation | LXC or VM | Same media volume for hardlinks with the download client |
| NAS | VM (common) | Optional passthrough disk for shares |
| Books | LXC or VM | **Own disk**; do not share the media library disk |

## 4. Network (generic)

- Private LAN only unless you add a reverse proxy + TLS intentionally.
- Give each guest a **static address you choose**.
- Document your map **privately** (password manager / Homelab wiki) — not in public git.

Example placeholder table (fill locally, never commit real values to a public repo):

| Role | Placeholder |
|---|---|
| Hypervisor UI | `https://<PROXMOX_HOST>:8006` |
| Media player | `http://<JELLYFIN_HOST>:8096` |
| Request UI | `http://<MEDIA_STACK_HOST>:5055` |
| Books reader | `http://<BOOKS_HOST>:5000` |

## 5. Storage philosophy

| Data | Where | Why |
|---|---|---|
| Hypervisor OS | Boot device | Keep small and rebuildable |
| Guest virtual disks | Guest storage pool | Snapshots/backups |
| Movies / TV / torrent completes | Dedicated media volume | Large, shared by player + automation |
| Books / textbooks | Books guest disk | Isolation from media server |

**Mount media volumes by LABEL or `/dev/disk/by-id/...`**, never by unstable `/dev/sdX` letters.

## 6. How stacks connect

```text
                    +------------------+
                    |   Homelab host   |
                    |    (Proxmox)     |
                    +--------+---------+
                             |
        +--------------------+--------------------+
        |                    |                    |
        v                    v                    v
 +--------------+    +---------------+    +--------------+
 | Media player |    | Media stack   |    | Books stack  |
 | (Jellyfin)   |    | (request/*arr)|    | (Kavita/...) |
 +------+-------+    +-------+-------+    +------+-------+
        ^                    |                   |
        |          shared media volume           |
        +--------------------+                   |
                             |            own disk only
                             v
                    +----------------+
                    | Media files    |
                    +----------------+
```

## 7. Related projects

- **Media stack** — creation + user guides for playback/automation
- **Books stack** — creation + user guides for reading libraries

## 8. Out of scope for this repo

- Exact package versions pinned to one build
- Router/firewall vendor configs
- Credentials, API keys, personal inventory
- Public exposure / Cloudflare tunnels (document separately if you add them)
