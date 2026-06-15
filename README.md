# Homelab Ansible Automation

Ansible automation for managing my homelab infrastructure, services, and monitoring systems.

## Infrastructure

Current environment consists of:

* Pi-hole DNS Server
* Uptime Kuma Monitoring
* GNS3 Lab Environment
* Ansible Control Node
* Additional Linux Servers

## Repository Structure

```text
ansible/
├── inventory.example.yml
├── group_vars/
│   └── all.example.yml
├── host_vars/
├── playbooks/
├── roles/
├── .gitignore
└── README.md
```

## Prerequisites

### Install Ansible

Ubuntu / Debian:

```bash
sudo apt update
sudo apt install ansible -y
```

Verify installation:

```bash
ansible --version
```

## SSH Authentication

This project uses SSH key authentication.

Generate a key pair if needed:

```bash
ssh-keygen -t ed25519
```

Copy the public key to managed hosts:

```bash
ssh-copy-id user@server-ip
```

Verify SSH access:

```bash
ssh user@server-ip
```

## Inventory Configuration

Copy the example inventory:

```bash
cp inventory.example.yml inventory.yml
```

Update host IP addresses and hostnames to match your environment.

Example:

```yaml
all:
  children:

    monitoring:
      hosts:
        kuma:
          ansible_host: 192.168.x.x

    services:
      hosts:
        pihole:
          ansible_host: 192.168.x.x

    infrastructure:
      hosts:
        gns3:
          ansible_host: 192.168.x.x

        ansible:
          ansible_host: 192.168.x.x
```

## Variables

Create your local variables file:

```bash
cp group_vars/all.example.yml group_vars/all.yml
```

Example:

```yaml
ansible_ssh_private_key_file: ~/.ssh/id_ed25519
```

Host-specific variables can be placed in:

```text
host_vars/
├── pihole.yml
├── kuma.yml
├── gns3.yml
└── server.yml
```

Example:

```yaml
ansible_user: ansible
```

## Testing Inventory

Validate inventory:

```bash
ansible-inventory -i inventory.yml --list
```

Display inventory graph:

```bash
ansible-inventory -i inventory.yml --graph
```

Show host variables:

```bash
ansible-inventory -i inventory.yml --host pihole
```

## Connectivity Testing

Test all hosts:

```bash
ansible all -i inventory.yml -m ping
```

Test a specific host:

```bash
ansible pihole -i inventory.yml -m ping
```

Test a group:

```bash
ansible infrastructure -i inventory.yml -m ping
```

Expected result:

```text
SUCCESS => {
    "ping": "pong"
}
```

## Ad-Hoc Commands

Run a command on all hosts:

```bash
ansible all -i inventory.yml -a "hostname"
```

Gather facts:

```bash
ansible all -i inventory.yml -m setup
```

Check uptime:

```bash
ansible all -i inventory.yml -a "uptime"
```

## Playbooks

Run a playbook:

```bash
ansible-playbook -i inventory.yml playbooks/update.yml
```

Examples:

### Update Systems

```bash
ansible-playbook -i inventory.yml playbooks/update.yml
```

### Reboot Systems

```bash
ansible-playbook -i inventory.yml playbooks/reboot.yml
```

## Privilege Escalation

Administrative tasks should use:

```yaml
become: true
```

Example:

```yaml
- hosts: all
  become: true

  tasks:
    - name: Update package cache
      apt:
        update_cache: true
```

## Security

This repository does not contain:

* Private SSH keys
* Passwords
* API tokens
* Production inventories
* Sensitive configuration

Ensure the following files are ignored:

```text
inventory.yml
group_vars/all.yml
host_vars/*.yml
```

## Git Workflow

Check repository status:

```bash
git status
```

Stage changes:

```bash
git add .
```

Commit:

```bash
git commit -m "Initial homelab automation setup"
```

Push:

```bash
git push origin main
```

## Future Improvements

* Docker automation
* Pi-hole management role
* Uptime Kuma deployment role
* Automated backups
* Monitoring stack deployment
* Proxmox automation
* Network automation (CCNA lab)

## License

MIT License
