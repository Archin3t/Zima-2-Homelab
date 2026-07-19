# Creation Guide — Homelab Blueprint

How to stand up the **hypervisor guests**. App installs are in the media/books repos.

## 0. Before you start

- [ ] Hypervisor installed (Proxmox VE or equivalent)
- [ ] Guest storage pool ready
- [ ] Media disk labeled and mountable by LABEL/by-id
- [ ] Private notes file for **your** IPs, IDs, and passwords (not in git)

## 1. Create guests (examples)

Resource numbers are starting points — adjust for your hardware.

### Media player guest

| Setting | Example |
|---|---|
| Type | LXC (Debian) or VM |
| vCPU | 2 |
| RAM | 2–4 GB |
| Disk | 8–16 GB OS |
| Extra | Bind-mount media volume read/write; pass `/dev/dri` if using VAAPI |
| Boot | Auto-start on host boot |

### Media automation guest

| Setting | Example |
|---|---|
| Type | LXC (Debian) with nesting if using Docker |
| vCPU | 2 |
| RAM | 2–4 GB |
| Disk | 8–16 GB OS |
| Extra | **Same** media volume mount as the player (for hardlinks) |

### NAS guest (optional)

| Setting | Example |
|---|---|
| Type | VM |
| vCPU | 1–2 |
| RAM | 1–2 GB |
| OS disk | 16–32 GB |
| Data | Passthrough or virtio disk for shares |

### Books guest

| Setting | Example |
|---|---|
| Type | LXC with nesting (Docker) or VM |
| vCPU | 2 |
| RAM | 2–4 GB |
| Disk | **64–256 GB** on guest storage (library lives here) |
| Extra | Do **not** mount the movies/TV volume unless you deliberately want to |

## 2. Networking

1. Attach guests to your LAN bridge.
2. Set static IPs **you** choose (DHCP reservation or guest static config).
3. Confirm each guest can reach the others you need (player ↔ automation; automation ↔ download client).

## 3. Media volume

On the host:

1. Create filesystem; set a stable LABEL.
2. Mount to a host path of your choosing (example: `/mnt/media`).
3. Bind-mount into player + automation guests at the **same absolute path** inside both (example: `/data/media`) so hardlinks work.

## 4. Next steps

1. Follow **homelab-media-stack** `CREATION-GUIDE.md` on the automation + player guests.
2. Follow **homelab-books-stack** `CREATION-GUIDE.md` on the books guest.
3. Store credentials in a password manager (see README FAQ / Homelab notes — recommended: Vaultwarden on LAN).

## 5. Backup sketch

| What | Idea |
|---|---|
| Guest configs / disks | Hypervisor backup jobs |
| Media library | Separate backup — often excluded from guest backups if bind-mounted |
| Books library | Backup the books guest disk or sync library folder off-box |
| Secrets | Password manager export / encrypted backup — never plain git |
