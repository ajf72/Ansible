# Lab Setup (Hypervisor) – Ansible Automation Training

This guide helps you build a reproducible lab environment to test the playbooks in this repository using a hypervisor (VMware, Hyper-V, VirtualBox, Proxmox, etc.).

## 1) Lab overview

You will create:

- 1x Control Node (Linux VM with Ansible installed)
- 2x Linux targets (Ubuntu Server)
- 1x Web server target (Ubuntu Server)
- Optional: 1x Cisco router (physical, GNS3, EVE-NG, or CML)

Ansible connects to Linux targets over SSH.

## 2) Recommended VM specs

**Control Node (Ubuntu)**
- CPU: 2 vCPU
- RAM: 2–4 GB
- Disk: 20–40 GB
- NIC: 1 (Lab network)

**Targets (Ubuntu Server)**
- CPU: 1–2 vCPU
- RAM: 1–2 GB
- Disk: 10–20 GB
- NIC: 1 (Lab network)

## 3) Network design

### Option A (recommended): Host-Only / Internal network
Use a dedicated lab network in your hypervisor.

Example addressing:

- Lab network: 192.168.56.0/24
- Control node: 192.168.56.10
- Linux 1: 192.168.56.20
- Linux 2: 192.168.56.21
- Web server: 192.168.56.30

### Option B: NAT network
Works fine too, but keep IPs stable (static or DHCP reservations).

## 4) OS choice

Recommended:
- Ubuntu Server 22.04 LTS (targets + control node)

Targets must have:
- OpenSSH Server enabled
- Python available (usually present on Ubuntu)

## 5) Prepare the Control Node

### Install Ansible
~~~bash
sudo apt update
sudo apt install -y ansible openssh-client
ansible --version
~~~

### Generate SSH key (if you don’t have one yet)
~~~bash
ssh-keygen -t ed25519 -C "ansible-lab" -f ~/.ssh/id_ed25519
~~~

## 6) Configure SSH access to targets

Copy your public key to each target (replace IPs):

~~~bash
ssh-copy-id student@192.168.56.20
ssh-copy-id student@192.168.56.21
ssh-copy-id student@192.168.56.30
~~~

Test login without password:

~~~bash
ssh student@192.168.56.20
~~~

## 7) Update inventory in this repo

Edit: `inventory/hosts.ini`

Example:

~~~ini
[linux]
192.168.56.20 ansible_user=student
192.168.56.21 ansible_user=student

[web]
192.168.56.30 ansible_user=student
~~~

## 8) Run the training

From the repo root:

### Connectivity test
~~~bash
ansible-playbook playbooks/ping.yml
~~~

### Update Linux targets
~~~bash
ansible-playbook playbooks/update.yml -K
~~~

### Deploy webserver
~~~bash
ansible-playbook playbooks/webserver.yml -K
~~~

Validate in browser:

- http://192.168.56.30

## 9) Cisco router (optional)

You can use:
- Physical Cisco router
- GNS3 / EVE-NG / CML

Make sure:
- SSH is enabled on the device
- The device is reachable from the control node

Example inventory:

~~~ini
[router]
192.168.56.1 ansible_user=cisco
~~~

## 10) Troubleshooting

### SSH fails
- Check connectivity from control node
- Confirm SSH service: `sudo systemctl status ssh`
- Confirm correct user/IP

### Ansible ping fails
~~~bash
ansible -m ping all
~~~

### Web page not visible
~~~bash
sudo systemctl status apache2
sudo ufw status
~~~

## 11) Snapshot workflow (recommended)

After installing OS + SSH:
- Create a snapshot called `clean-install`
- Clone new targets from that snapshot

This keeps your lab consistent and fast.
