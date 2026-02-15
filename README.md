# Ansible Automation Training

![Ansible](https://img.shields.io/badge/Ansible-Automation-red?logo=ansible)
![IaC](https://img.shields.io/badge/IaC-Infrastructure--as--Code-blue)
![DevOps](https://img.shields.io/badge/DevOps-Principles-purple)
![Status](https://img.shields.io/badge/status-training-green)

A practical hands-on training repository to learn Infrastructure as Code using Ansible.

## What is Ansible?

Ansible is an automation tool used to configure systems, deploy applications and manage infrastructure using YAML-based playbooks.

It works agentless over SSH and follows an idempotent model: running the same playbook multiple times should not keep making unnecessary changes.

## Repository structure

~~~text
inventory/   -> Target systems definition
playbooks/   -> Automation logic
templates/   -> Jinja2 templates
docs/        -> Assignment & rubric
ansible.cfg  -> Ansible configuration
~~~

## How it works

1. **Inventory** defines your targets (hosts/groups).
2. **Playbooks** define the desired state (tasks).
3. **Modules** (apt, service, ios_config) implement actions safely and idempotently.
4. Ansible connects via **SSH** (Linux) and network modules (Cisco).

## Quick start

### 1) Validate connectivity

~~~bash
ansible-playbook playbooks/ping.yml
~~~

Tests SSH connectivity and confirms Ansible can run tasks on the target.

### 2) Update Linux systems

~~~bash
ansible-playbook playbooks/update.yml -K
~~~

Updates apt cache and upgrades installed packages.

### 3) Deploy a webserver

~~~bash
ansible-playbook playbooks/webserver.yml -K
~~~

Installs Apache and deploys a Jinja2-based index page.

### 4) Configure a Cisco router

~~~bash
ansible-playbook playbooks/router.yml
~~~

Applies router configuration using `ios_config`.

### 5) Create a router backup

~~~bash
ansible-playbook playbooks/backup.yml
~~~

Creates a configuration backup of the router.

## Requirements

- Ansible installed on a control node
- SSH access to Linux targets
- Cisco device (real or simulated)
- Python available on Linux targets

## Training material

- Assignment: `docs/training-assignment.md`
- Rubric: `docs/rubric.md`
- Architecture: `docs/architecture.md`

## Documentation
See the full documentation in the [docs folder](docs/README.md).
