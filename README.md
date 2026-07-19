# ZimaBoard 2 Homelab Blueprint

Public documentation for a **small Proxmox-based Homelab**: media streaming, optional NAS, and a separate books/reading stack.

In my case I am utilizing the ZimaBoard 2 1664 based on x86 Architecture. The following are the contents/specifications of the ZimaBoard 2 1664:

- Intel N150 Quad-Core up to 3.6GHz
- 16GB LPDDR5 4800MHz RAM
- 64GB eMMC Storage
- 2× SATA Ports
- 2× 2.5GbE
- 2× USB 3.0
- 1× Mini DisplayPort

This repository describes **architecture and roles**. Application install details live in companion repos:

- [homelab-media-stack](https://github.com/Archin3t/homelab-media-stack)
- [homelab-books-stack](https://github.com/Archin3t/homelab-books-stack)

## Docs

| File(s) | Purpose |
|----------|---------|
| [docs/DOCUMENTATION.md](docs/DOCUMENTATION.md) | System blueprint & design |
| [docs/CREATION-GUIDE.md](docs/CREATION-GUIDE.md) | How to create the hypervisor guests |
| [docs/USER-GUIDE.md](docs/USER-GUIDE.md) | Which service to open for which job |
| [docs/SECURITY](docs/SECURITY.md) | Security Policy & Practices |
| [site/index.html](Static-Site-View/index.html) | Single-page HTML manual |

## Safety

This project is intentionally **free of secrets**: no passwords, API keys, personal usernames, or private LAN addresses. Use placeholders and set real values only on your own infrastructure.

I try to still show how a user like you could potentially have the system laid out, for example IP Scheme/Subnets, Ports and IP addresses, Usernames and Passwords, Etc.

## License

ZIMA Has its own Privacy Policy & Agreements, I am affiliated however do not share agreements, or have any binding contracts.

Documentation provided as-is for educational / Homelab use. Not affiliated with Proxmox, Jellyfin, or related projects. (Excluding ZIMA)

Do not take this/make content of this repository without crediting me first:

- [Instagram](https://www.instagram.com/archin3t)
- [YouTube](https://www.youtube.com/@Archinet-Labs)
- [GitHub](https://github.com/Archin3t)
