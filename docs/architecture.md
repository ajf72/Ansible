# Architecture Overview

## Components

- **Control Node**: the machine where Ansible is installed.
- **Managed Nodes**: Linux servers and network devices defined in the inventory.

## Communication

- Linux hosts: SSH
- Cisco devices: network modules (e.g. `ios_config`) over SSH

## Automation flow

Control Node -> Inventory -> Playbook -> Module -> Target System
