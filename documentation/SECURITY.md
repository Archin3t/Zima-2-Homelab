# Security Policy — ZimaBoard 2 Homelab Blueprint

This repository is **public documentation**.

Its purpose is to document architecture, design decisions, and educational homelab practices.

It must never contain secrets, private infrastructure details, or information that could assist unauthorized access.

---

# What Belongs in This Repository

This repository may contain:

- Architecture documentation
- System diagrams
- Role separation concepts
- Generic configuration examples
- Placeholder values
- Educational homelab guidance
- Public-safe deployment patterns

Approved examples:

```text
<PROXMOX_HOST>
<JELLYFIN_HOST>
<MEDIA_STACK_HOST>
<BOOKS_HOST>
<NAS_HOST>
```

Example paths and ports are acceptable when they do not expose private infrastructure.

Examples:

```text
/mnt/media
/mnt/books
:8096
:8006
```

---

# What Must Stay Private

Never commit the following:

## Credentials

- Passwords
- Default credentials
- API keys
- Access tokens
- Authentication secrets

---

## Infrastructure Details

Keep private:

- Real LAN IP addresses
- Public DNS records
- Private hostnames
- Usernames
- Hardware inventory
- Network diagrams containing sensitive information

---

## Security Files

Never publish:

- `.env` files containing secrets
- SSH private keys
- TLS certificates
- Private keys
- Credential files
- Password manager exports

Examples:

```text
.env
.pem
.key
credentials.json
secrets.yaml
```

---

## Screenshots

Before uploading screenshots, verify they do not reveal:

- Login credentials
- Tokens
- Private URLs
- Internal IP addresses
- Personal information
- Network topology details

---

# Public Documentation Rules

When documenting your setup:

Use:

```text
<USERNAME>
<PASSWORD>
<SERVICE_HOST>
<DOMAIN>
```

Do not use:

```text
admin
password123
192.168.x.x
your-real-domain.com
```

---

# Before Every Commit

Review changes before pushing.

Checklist:

- [ ] No real IP addresses or private DNS entries
- [ ] No passwords or default credentials
- [ ] No API keys or tokens
- [ ] No `.env` files staged
- [ ] No private certificates or key files
- [ ] No screenshots containing sensitive information
- [ ] Companion repository links point only to public-safe resources

---

# If a Secret Is Accidentally Published

Assume the secret is compromised.

Immediately:

1. Rotate or revoke the exposed credential.
2. Remove the secret from the repository.
3. Remove it from Git history if it has been pushed publicly.
4. Review related services for unauthorized access.
5. Replace the credential before continuing normal operation.

Removing a file from the latest commit does not guarantee it has been removed from Git history.

---

# Responsible Reporting

If you discover sensitive information inside this repository:

Please do not publicly share it.

Contact the repository owner privately with:

- The affected file
- The type of information exposed
- Any relevant details needed to locate it

---

# Documentation Philosophy

This project intentionally separates:

## Public Information

- Architecture
- Design patterns
- Deployment concepts
- Educational guidance

## Private Information

- Credentials
- Personal infrastructure
- Network details
- Access information

The goal is to provide a useful homelab blueprint without exposing anyone's environment.

---

# Credits & Attribution

Created and maintained by **Archin3t**

This security policy is part of the **ZimaBoard 2 Homelab Blueprint** documentation project.

If this documentation structure, security practices, or related materials are referenced elsewhere, visible credit and a link back to this repository are appreciated.

---

© 2026 Archin3t. All Rights Reserved.
