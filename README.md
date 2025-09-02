# Ansible Role: cloudflared-DOH

[![CI](https://github.com/HomeSecExplorer/ansible-role-cloudflared/actions/workflows/ci.yml/badge.svg)](https://github.com/HomeSecExplorer/ansible-role-cloudflared/actions/workflows/ci.yml)
![Ansible Galaxy](https://img.shields.io/badge/ansible-galaxy-blue?logo=ansible)
![License: MIT](https://img.shields.io/badge/license-MIT-green.svg)

---

**Author:** HomeSec Explorer  
**License:** MIT  
**Tags:** cloudflared, dns, doh, proxy, systemd, pihole, privacy, selfhosted

## Description

This role installs and configures [cloudflared](https://developers.cloudflare.com/cloudflared/) as a DNS-over-HTTPS (DoH) proxy service.

It supports both installation via Debian package repository or GitHub release (default). The role is ideal for use with [Pi-hole](https://pi-hole.net/) or alongside the [`HomeSecExplorer.pihole`](hhttps://github.com/HomeSecExplorer/ansible-role-pihole) role for enhanced privacy and DNS filtering.

Each cloudflared service instance runs on its own port and configuration, allowing multiple upstreams or configurations per instance.

---

## Requirements

- Ansible `>= 2.13`
- Root privileges on the target host
- Internet access for downloading and running cloudflared

---

## Supported Operating Systems

- Debian 11 (Bullseye), 12 (Bookworm), 13 (Trixie)
- Ubuntu 22.04 (Jammy), 24.04 (Noble)
- Rocky 9 (Blue Onyx)

> ⚠️ The OS compatibility check (`hsecd_os_check`) ensures supported platforms for this role - not official Cloudflare support. Installation on ARM devices is **not officially tested**.

## Test matrix

**Legend:** :white_check_mark: manual test passed - :repeat: covered in CI - :white_circle: not tested

| Distro | Version | Manually verified | CI | Notes |
|:-------|:--------|:-----------------:|:--:|:-----|
| Debian | 13 | :white_check_mark: | :repeat: |  |
| Debian | 12 | :white_check_mark: | :repeat: |  |
| Debian | 11 | :white_circle: | :repeat: |  |
| Ubuntu | 24.04 | :white_check_mark: | :repeat: |  |
| Ubuntu | 22.04 | :white_circle: | :repeat: |  |
| Rocky | 9 | :white_check_mark: | :repeat: |  |

---

## Role Variables (Examples)

> ⚙️ Full list in `defaults/main.yml`

### Installation & Configuration

```yaml
hsecd_install: true               # Install cloudflared
hsecd_uninstall: false            # Uninstall and remove cloudflared
hsecd_update: true                # Run cloudflared update (only if method is 'git')
hsecd_auto_update: false          # Enable cron-based updates (Git install only)
hsecd_configure: true             # Configure service files and systemd units
hsecd_os_check: true              # Abort on unsupported OS
hsesp_arch_check: true            # Enforce architecture check (ARMv6 workaround)

hsecd_install_method: 'git'       # 'git' or 'repo'
hsecd_git_tag: 'latest'           # GitHub release tag (e.g. 'latest' or '2025.0.1')
```

### Service Instances

```yaml
hsecd_cloudflared_auto_port: true         # Automatically assign ports
hsecd_cloudflared_port_base: 5053         # First service gets 5053, next 5054, ...

hsecd_cloudflared_services:
  - upstreams:
      - 'https://1.1.1.1/dns-query'
      - 'https://1.0.0.1/dns-query'
    options: '--address 127.0.0.1'
    # port: 5053 (optional if auto_port is true)

  - upstreams:
      - 'https://9.9.9.9/dns-query'
      - 'https://149.112.112.112/dns-query'
    options: '--address 127.0.0.1'
```

Each entry creates a separate systemd service (e.g. `cloudflared1.service`, `cloudflared2.service`) and an `/etc/default/cloudflaredX` file containing the corresponding `CLOUDFLARED_OPTS` line..

---

## Available Tags

- `install` – Install cloudflared from repo or GitHub
- `uninstall` – Remove cloudflared and all associated files
- `update` – Run `cloudflared update` (only for Git installs)
- `config` – Create/update systemd units and config files

---

## Install this role

From Ansible Galaxy:

```bash
ansible-galaxy install HomeSecExplorer.cloudflared
```

Or manually (via Git):

```bash
git clone https://github.com/HomeSecExplorer/ansible-role-cloudflared.git roles/HomeSecExplorer.cloudflared
```

---

## Example Playbook

```yaml
- name: Set up cloudflared as DoH service
  hosts: all
  become: true
  roles:
    - role: HomeSecExplorer.cloudflared
```

---

## Recommended Pairing

This role works especially well alongside:

- [`HomeSecExplorer.pihole`](hhttps://github.com/HomeSecExplorer/ansible-role-pihole) – install and configure Pi-hole
- Use cloudflared as Pi-hole’s upstream DNS over HTTPS provider

---

## License

MIT

---

## Author Information

HomeSec Explorer  
🔗 [YouTube Channel](https://www.youtube.com/@HomeSecExplorer)

If this role was helpful, drop a ⭐ on GitHub, subscribe on YouTube or Sponsor me!
