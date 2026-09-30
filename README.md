# VPS Server Hardening & Baseline Automation with Ansible

Automated Infrastructure Configuration Management for Ubuntu 24.04 LTS VPS instances, engineered by Muhammad Nur Ashiddiqi.

## Overview
This repository provides an idempotent Ansible playbook designed to provision and enforce production-grade security baselines on freshly created Linux VPS instances.

### Key Security Implementations:
1. **Automated Package Management**: Updates system caches and provisions core tools (`curl`, `git`, `ufw`, `fail2ban`, `htop`, `jq`).
2. **SSH Hardening**:
   - Disables direct root login (`PermitRootLogin no`).
   - Disables password-based authentication (`PasswordAuthentication no`), enforcing cryptographic SSH keypairs.
   - Restricts brute-force authentication attempts (`MaxAuthTries 3`).
   - Automatic configuration validation before restarting the SSH daemon.
3. **Network Firewall (UFW)**:
   - Sets strict default-deny policy for incoming traffic (`default: deny`).
   - Opens only explicit production service ports (`22/tcp`, `80/tcp`, `443/tcp`).
4. **Intrusion Prevention (Fail2Ban)**:
   - Enforces automated IP jailing for repetitive connection anomalies.

---

## Architecture & File Structure

```text
├── ansible.cfg       # Global Ansible configurations & YAML output formatting
├── inventory.ini     # Target server definitions and environment variables
├── playbook.yml      # Declarative tasks and security handlers
└── README.md         # Production documentation
```

---

## Quickstart

### 1. Prerequisites
- Python 3.10+
- Ansible Core 2.15+

```bash
# Clone repository
git clone https://github.com/Tnembull/ansible-vps-hardening.git
cd ansible-vps-hardening

# Setup virtual environment
python3 -m venv .venv
source .venv/bin/activate
pip install ansible-core
ansible-galaxy collection install community.general
```

### 2. Configure Inventory
Update `inventory.ini` with your target VPS IP address:

```ini
[vps_servers]
target_vps ansible_host=YOUR_VPS_IP ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/id_rsa
```

### 3. Dry-Run / Pre-Flight Verification
Run in check mode to preview changes without modifying server state:

```bash
ansible-playbook -i inventory.ini playbook.yml --check
```

### 4. Execute Playbook
Apply configurations across all target servers:

```bash
ansible-playbook -i inventory.ini playbook.yml
```

---

## Idempotency Verification
Ansible tasks are fully idempotent. Re-running the playbook on an already hardened node reports:
```text
localhost : ok=7 changed=0 unreachable=0 failed=0
```
No redundant writes or service interruptions occur.

---
**Author**: Muhammad Nur Ashiddiqi  
**Portfolio**: [muhammadnurashiddiqi.my.id](https://muhammadnurashiddiqi.my.id)  
**Role**: DevOps Engineer
