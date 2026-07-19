# Security policy

This repository is **public documentation**. It must never contain secrets, credentials, or infrastructure-specific identifiers that could aid unauthorized access.

## What belongs here

- Architecture descriptions and role separation
- Placeholder hostnames such as `<PROXMOX_HOST>`, `<JELLYFIN_HOST>`, `<MEDIA_STACK_HOST>`, `<BOOKS_HOST>`
- Example ports, paths, and generic configuration patterns
- Educational homelab guidance

## What must stay private

Store these only on your servers, in a password manager, or in a **private** notes repo — never in this public tree:

- Passwords, API keys, tokens, and session cookies
- Private LAN IP addresses (e.g. `10.x`, `192.168.x`)
- Personal usernames, hostnames, or domain names tied to your identity
- VPN keys, TLS private keys, SSH private keys, and `.env` files with real values
- Screenshots or exports that embed credentials

## Placeholders only

Documentation uses angle-bracket placeholders. Replace them locally when you deploy. Do not commit real values “just for convenience” — git history retains them even after deletion.

## `.gitignore` is not security

Ignoring `.env`, `*.pem`, or `secrets/` prevents **accidental** commits. It does **not**:

- Remove secrets already pushed to git history
- Stop someone with repo access from adding ignored files locally and copying them elsewhere
- Replace code review, pre-commit hooks, or secret scanning

Treat `.gitignore` as a safety net, not a vault.

## Redact checklist (before every commit / PR)

- [ ] No real IP addresses or DNS names for your LAN
- [ ] No passwords, passphrases, or default credentials left unchanged
- [ ] No API keys (Sonarr, Radarr, Prowlarr, Jellyfin, indexer, etc.)
- [ ] No personal usernames or machine hostnames
- [ ] No `.env`, `.pem`, `.key`, or `credentials*` files staged
- [ ] No screenshots with visible logins or URLs containing secrets
- [ ] Compose/config examples use placeholders or clearly fake values
- [ ] Run a secret scan or manual search if you edited many files

## If you accidentally commit a secret

1. **Rotate/revoke** the exposed credential immediately on the live service.
2. Remove the secret from the working tree.
3. Rewrite history or use your platform’s secret-removal tooling if the commit was pushed.
4. Do not assume deletion alone is sufficient — assume compromise until rotated.

## Reporting

This is a personal/educational documentation project. If you find sensitive data that should not be public, open an issue or contact the repository owner privately so it can be removed and rotated.
