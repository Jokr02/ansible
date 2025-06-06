# Homelab Ansible Automation

Dieses Repository enthält die vollständige Ansible-Infrastruktur zur Verwaltung und Automatisierung von System-Upgrades und Benachrichtigungen in meinem Homelab.

## 📂 Projektstruktur

```bash
.
├── ansible.cfg          # Ansible Konfiguration
├── inventory/           # Host-Inventory (nur IPs / Hostnamen, keine Secrets!)
│   └── hosts
├── playbooks/           # Ansible Playbooks
│   └── update_lxc.yml   # Paket-Upgrade Playbook mit Discord-Benachrichtigung
└── roles/               # Wiederverwendbare Rollen
    └── discord_notify/  # Enterprise-Discord-Benachrichtigungsrolle
        └── tasks/
            └── main.yml
