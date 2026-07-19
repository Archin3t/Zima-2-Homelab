# User Guide — ZimaBoard 2 Homelab

This guide covers everyday use and basic troubleshooting.

The goal is simple:

**Know which service to open, what each part does, and where to look when something goes wrong.**

Keep your real:

- IP addresses
- Hostnames
- Credentials
- Service URLs

in private notes or a password manager.

Do not store personal infrastructure details in public repositories.

---

# Quick Access Doors

| I want to... | Open |
|--------------|------|
| Watch movies and TV | `http://<JELLYFIN_HOST>:8096` |
| Request a movie or show | `http://<MEDIA_STACK_HOST>:5055` |
| Check downloads | `http://<MEDIA_STACK_HOST>:8080` |
| Manage TV automation | Sonarr `:8989` |
| Manage movie automation | Radarr `:7878` |
| Manage indexers | Prowlarr `:9696` |
| Read books | `http://<BOOKS_HOST>:5000` |
| Manage book ingestion | `http://<BOOKS_HOST>:8083` |
| Manage NAS shares | `http://<NAS_HOST>` |
| Manage virtual machines | `https://<PROXMOX_HOST>:8006` |

---

# Mental Model

Think of the homelab as separate rooms.

| Component | Purpose |
|-----------|---------|
| Jellyfin | Watch your media |
| Jellyseerr | Request new movies and shows |
| Sonarr / Radarr | Organize and manage libraries |
| Prowlarr | Manage indexers and sources |
| qBittorrent | Download client |
| Kavita | Read books and comics |
| Calibre-Web Automated | Book library management |
| OpenMediaVault | File shares and storage |
| Proxmox | Runs and manages everything |

---

# Common Workflows

## Watching Movies or TV

```text
Jellyfin
   |
   v
Media Library
   |
   v
Playback
```

Open Jellyfin and select your content.

If content is missing:

1. Confirm the file exists.
2. Confirm Jellyfin has scanned the library.
3. Check the automation stack.

---

## Requesting New Content

```text
Jellyseerr
      |
      v
Sonarr / Radarr
      |
      v
qBittorrent
      |
      v
Import + Organize
      |
      v
Jellyfin Update
```

Normally:

1. Request the title in Jellyseerr.
2. Sonarr/Radarr manages the request.
3. qBittorrent downloads it.
4. The file is imported.
5. Jellyfin detects the update.

---

## Reading Books

```text
Book Import
      |
      v
Library Folder
      |
      v
Kavita Scan
      |
      v
Read
```

Books are intentionally separate from the movie and TV library.

---

# Troubleshooting

## Request Appears Stuck

### Check:

1. Jellyseerr request status
2. Sonarr/Radarr Activity
3. qBittorrent download status
4. Import history
5. Jellyfin library scan

---

## Missing Episodes

Check:

1. Sonarr series status
2. Sonarr → Search Missing
3. Download history
4. Import errors

A tracked show does not always mean every episode has already been acquired.

---

## Missing Movies

Check:

1. Radarr movie status
2. Radarr → Search Missing
3. Download client status
4. Import history

---

## Books Missing

Check:

1. Kavita library paths
2. Kavita scan status
3. Books guest storage
4. Calibre-Web Automated output folders

Confirm the files exist inside the configured books library.

---

## Guest Is Offline

Open:

```text
https://<PROXMOX_HOST>:8006
```

Check:

- Guest status
- CPU/RAM usage
- Storage availability
- Boot errors

Try:

1. Restart the guest.
2. Check recent logs.
3. Confirm storage mounts.

---

## Disk Disappeared After Reboot

Common cause:

Using unstable device paths.

Avoid:

```text
/dev/sdX
```

Prefer:

```text
LABEL=<NAME>
```

or:

```text
/dev/disk/by-id/
```

Verify:

- Disk detection
- Mount configuration
- Filesystem status

---

# Maintenance Habits

Recommended routine:

## Weekly

- Check Proxmox updates
- Check storage usage
- Verify backups
- Review failed downloads/imports

## Monthly

- Test restores
- Review logs
- Remove unused files
- Update documentation

---

# Safety Habits

Always:

- Keep credentials private
- Keep real IP addresses private
- Use stable disk identifiers
- Maintain backups
- Separate important data from temporary data

Never publish:

- Passwords
- API keys
- Tokens
- SSH keys
- Private network details

---

# Related Documentation

For installation and deeper configuration:

## Media Stack

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

**[ZimaBoard-NAS](https://github.com/Archin3t/ZimaBoard-NAS)**

Covers:

- Storage layouts
- OpenMediaVault
- Disk management
- Shares
- Backups
- Additional services

---

# Credits & Attribution

Created and maintained by **Archin3t**

This user guide is part of the **ZimaBoard 2 Homelab Blueprint** documentation project.

If this documentation, workflows, diagrams, or structure are referenced in:

- Videos
- Articles
- Tutorials
- Courses
- GitHub projects
- Social media content

please provide visible credit and link back to this repository.

Substantial reuse of this documentation requires permission.

---

© 2026 Archin3t. All Rights Reserved.
