# User Guide — Homelab Blueprint

Day-to-day: which door to open. Replace placeholders with **your** addresses (keep that list private).

## Quick doors

| I want to… | Open |
|---|---|
| Watch movies/TV | Media player → `http://<JELLYFIN_HOST>:8096` |
| Request new movie/show | Request UI → `http://<MEDIA_STACK_HOST>:5055` |
| Check torrent progress | Download client WebUI on the media-stack guest |
| Read books | Books reader → `http://<BOOKS_HOST>:5000` |
| Manage files/shares | NAS UI (if deployed) |
| Manage VMs/containers | Hypervisor UI → `https://<PROXMOX_HOST>:8006` |

## Mental model

- **Player** = watch  
- **Request UI** = ask for new titles  
- **Automation (*arr)** = find + file (usually ignore)  
- **Books reader** = read; separate from movies  

## Credentials

Use accounts **you** created. This public guide never includes passwords.

Recommended: store all Homelab logins in a **self-hosted password manager** on your LAN (e.g. Vaultwarden) so phones/PCs share vaults without putting secrets in git.

## When something is missing

| Problem | Where to look |
|---|---|
| New request not appearing in player | Download client → automation Activity → player library |
| Missing episodes of a show you already track | TV automation app → Search Missing (not the request UI) |
| Books not listed | Books reader libraries + scan; check library folders on books guest |
| Guest down | Hypervisor UI → start guest |

## Safety habits

- Do not commit real IPs/passwords to public repos  
- Prefer LABEL/by-id mounts for USB/SATA media disks  
- Keep books disk separate if you want media-drive isolation  
