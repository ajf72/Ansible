
# ⚙️ Ansible Setup voor Ubuntu + Windows Targets

Deze repository bevat een eenvoudige Ansible-configuratie waarmee je vanuit een Ubuntu control node zowel een Ubuntu-server als een Windows Server kunt aansturen.

## 🧰 Wat je nodig hebt

| Rol             | OS           | Functie                     |
|----------------|--------------|------------------------------|
| Control Node   | Ubuntu       | Met Ansible geïnstalleerd   |
| Linux Target   | Ubuntu       | Doelmachine via SSH         |
| Windows Target | Windows      | Doelmachine via WinRM       |

---

## 📦 Installatie op de control node

```bash
sudo apt update
sudo apt install ansible python3-pip -y
pip install "pywinrm>=0.3.0"
```

---

## 📁 Structuur

```bash
.
├── inventory.ini
├── ping-test.yml
└── README.md
```

---

## 📡 Inventory-bestand (inventory.ini)

```ini
[linux]
ubuntu-target ansible_host=192.168.1.101 ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/id_rsa

[windows]
windows-target ansible_host=192.168.1.102 ansible_user=Administrator ansible_password=P@ssw0rd123 ansible_connection=winrm ansible_winrm_transport=basic ansible_winrm_server_cert_validation=ignore
```

Pas IP-adressen, gebruikersnamen en wachtwoorden aan naar jouw omgeving.

---

## 🔧 Windows voorbereiden voor Ansible

Op de Windows Server voer je als administrator het volgende uit:

```powershell
winrm quickconfig
Enable-PSRemoting -Force
winrm set winrm/config/client '@{TrustedHosts="*"}'
```

---

## ✅ Test de verbinding

Ping de Linux target:

```bash
ansible -i inventory.ini linux -m ping
```

Ping de Windows target:

```bash
ansible -i inventory.ini windows -m win_ping
```

Ping beide:

```bash
ansible -i inventory.ini all -m ping
```

---

## 🧪 Extra: systeeminfo ophalen

```bash
ansible -i inventory.ini all -m setup
```

---

> Gemaakt voor testomgevingen. Beveilig je wachtwoorden en hosts bij productiegebruik.
