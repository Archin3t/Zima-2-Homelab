ZimaBoard 2 Portable Homelab — Archinet Labs

A Proxmox-based portable homelab built on the ZimaBoard 2.

Designed for virtualization, media streaming, NAS storage, cybersecurity labs, and secure remote access via Tailscale.

Maintained by Archinet Labs  
YouTube: https://www.youtube.com/@Archinet-Labs  
Instagram: https://www.instagram.com/archin3t  
LinkedIn: https://www.linkedin.com/in/declan-lebeau-073905389  
GitHub: https://github.com/Archin3t  
Affiliate Link (ZimaBoard 2): https://bit.ly/4eav6l1  

---

# Quick Access Services

| Service | URL |
|----------|-----|
| Proxmox VE | https://10.10.10.104:8006 |
| Plex Media Server | http://10.10.10.10:32400/web |
| OpenMediaVault (NAS) | https://10.10.10.11 |
| Netdata Monitoring | http://10.10.10.104:19999 |

---

# Architecture Overview

The system is built on a Proxmox VE hypervisor hosted on the ZimaBoard 2.

Core workloads include virtualization, media streaming, and network-attached storage, with strict isolation between services.

## Host System
**ZimaBoard 2 (Proxmox @ 10.10.10.104)**

```
├── CT 110  Plex + Tailscale        10.10.10.10   USB 4TB media (isolated)
├── VM 111  OpenMediaVault NAS      10.10.10.11   SATA 1TB (SMB/NFS)
├── VM 120  Windows 10 Target       10.10.10.20
├── VM 121  Kali Linux              10.10.10.21
├── VM 122  Parrot OS               10.10.10.22
└── VM 123  Android-x86             10.10.10.23
```

---

# Storage Design

Storage is intentionally separated to reduce contention and improve reliability:

- **SATA 1TB (sda):** Proxmox VM storage pool (disks, snapshots, backups)
- **SATA 1TB (sdb):** Dedicated OpenMediaVault passthrough disk
- **USB 4TB (sdc):** Plex media library (read-heavy workload isolation)
- **eMMC:** Proxmox host operating system

This design isolates media streaming, virtualization workloads, and NAS operations to prevent I/O contention.

---

# Network Design

- Subnet: **10.10.10.0/24**
- Static IP allocation for all services
- Internal-only service exposure (no direct WAN access)
- Tailscale overlay network for secure remote access
- Plex is the only externally reachable service (VPN-restricted)

---

# Security Model

This environment follows an isolation-first security design:

- Each VM has a defined role and trust boundary
- Media storage is isolated from compute workloads
- Security lab VMs are segmented from NAS and Plex
- No direct WAN exposure of internal services
- Tailscale ACLs enforce remote access control
- Snapshots used for rapid rollback in lab environments

---

# Operations

- Create snapshots before major configuration changes
- Perform weekly backups of NAS datasets (if secondary storage available)
- Monitor system health via Netdata
- Apply Proxmox and container updates on a controlled schedule
- Reset or isolate lab environments after testing sessions

---

# First-Time Setup Checklist

- Install and configure OpenMediaVault (VM 111)
  → Follow `docs/nas.md`

- Install Kali Linux (VM 121)
  → GUI setup via `docs/virtual-machines.md`

- Deploy Plex Media Server (CT 110)
  → Configure libraries and verify storage mounts

- Configure Tailscale on Plex container
  → Restrict access to VPN-only connections

- Install Windows 10 and Parrot OS VMs
  → Use ISO templates and snapshot baseline states

- Validate SMB/NFS shares from NAS VM
  → Test cross-VM and host connectivity

---

# Virtualization Lab Use Cases

- Windows exploitation testing (isolated environment)
- Linux privilege escalation practice (Kali / Parrot OS)
- Malware analysis using snapshot rollback
- Network scanning and segmentation testing
- Attack simulation against Windows 10 target VM

---

# Security Principles

- Portable: Deployable across compatible hardware
- Modular: Services can be swapped or scaled independently
- Reproducible: Fully documented VM and storage layout
- Secure by default: No public WAN exposure

---

# Project Identity

Archinet Labs focuses on:

- Portable homelabs
- Cybersecurity experimentation
- Edge computing infrastructure
- Self-hosted media and storage systems
