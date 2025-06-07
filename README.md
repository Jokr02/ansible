# Homelab Proxmox ISO & LXC Automation

This repository contains the fully automated Ansible playbooks for managing ISO downloads and LXC template updates for my Proxmox Homelab cluster.

---

## 🔧 What does it do?

- Automatically downloads latest Debian and Ubuntu ISO images for Proxmox
- Dynamically detects and downloads the latest LXC container templates for:
  - Ubuntu 22.04
  - Debian 12
- Sends a Discord notification after each run with full version information
- Fully automated via Semaphore scheduled templates

---

## 🖥 System Overview

- Host: Proxmox 8.x running on Ryzen 5 PRO 4650G
- Storage:
  - ISOs: `/var/lib/vz/template/iso`
  - LXC templates: `/var/lib/vz/template/cache`
  - NAS: OpenMediaVault RAID1 backend connected via NFS
- Automation fully driven by:
  - Ansible
  - Semaphore (running inside LXC)
  - Dockerized control layer
  - Discord integration

---

## ⚙ Technologies

- Ansible (roles-based structure)
- Semaphore (CI/CD for Ansible)
- Proxmox VE native `pveam` for LXC templates
- Discord Webhook integration for monitoring
- GitHub for version control

---

## 🚀 Playbook Structure

### Playbooks

- `playbooks/update_proxmox_templates.yml`  
  Entry point playbook for ISO & LXC updates.

### Roles

- `roles/download_proxmox_images/tasks/main.yml`  
  Handles:
  - ISO version management (safe static mapping)
  - Debian ISO extraction via HTTP parsing
  - Dynamic LXC template detection via Proxmox `pveam`
  - Discord reporting

---

## 📩 Discord Notifications

- Sends a Discord embed with version info after every successful run.
- Uses Semaphore Variable Group for secure webhook handling:

```yaml
discordwebhook: https://discord.com/api/webhooks/xxxx
Example notification content:

json
Kopieren
{
  "username": "Proxmox ISO Downloader",
  "embeds": [
    {
      "title": "✅ Proxmox ISO & LXC Templates updated",
      "color": 65280,
      "fields": [
        {"name": "Ubuntu ISO", "value": "ubuntu-24.04.2-live-server-amd64.iso"},
        {"name": "Debian ISO", "value": "debian-12.5.0-amd64-netinst.iso"},
        {"name": "Ubuntu LXC", "value": "ubuntu-22.04-standard_20240601_amd64.tar.zst"},
        {"name": "Debian LXC", "value": "debian-12-standard_20240601_amd64.tar.zst"}
      ],
      "timestamp": "2025-06-07T09:30:00Z"
    }
  ]
}
🔄 Scheduling
The playbook is scheduled via Semaphore:

Frequency: Weekly

Day: Sunday

Time: 03:00 local time (UTC+2)

Cron expression (UTC for Semaphore configuration):

cron
Kopieren
0 1 * * 0
✅ Current Stability
Task	Status
ISO Download	✅ Stable
LXC Template Download	✅ Dynamic
Discord Notification	✅ Working
Semaphore Integration	✅ Active
Scheduling	✅ Automated

🧪 Planned Improvements
Proxmox API integration for full VM/LXC management

Cleanup logic for old ISOs & LXC templates

Automated backups using Proxmox Backup Server

Enhanced monitoring with Grafana, Prometheus, Loki, etc.

Secure secrets handling via Ansible Vault

Self-healing container automation

GitOps deployment structure

🔒 Security Notes
All credentials and secrets are managed securely via Semaphore Variables.

No sensitive information is stored inside this repository.

🏷 Author
Built for my personal Homelab Automation Stack 🚀

📂 Role Documentation (download_proxmox_images)
Role Purpose
Automates Proxmox ISO & LXC template management and monitoring.

Key Features
Static safe mapping for Ubuntu ISOs

Dynamic detection of latest LXC templates via pveam

Discord embed notifications after each update run

Fully compatible with Semaphore CI/CD pipelines

Role Variables
No internal variables required.

discordwebhook must be provided externally via Semaphore Variables.

Used Directories
/var/lib/vz/template/iso for ISO files

/var/lib/vz/template/cache for LXC templates

Requirements
Proxmox pveam CLI available on host

Ansible 2.10+ (recommended)

Semaphore CI configured for scheduled execution

Compatibility
Proxmox 7.x / 8.x

Ubuntu 22.04 / Debian 12 Homelab systems
