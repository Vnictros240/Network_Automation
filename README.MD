# 🚀 Network Automation with Ansible (Cisco IOS-XE)

Welcome to the **Network Automation Lab** — a practical, production-minded repository for automating Cisco IOS-XE environments using Ansible.

This repo is designed for **network engineers transitioning into automation and cybersecurity**, focusing on repeatable workflows, safe change management, and scalable infrastructure control.

---

## 🎯 Objectives

* Automate Cisco IOS-XE device configuration
* Standardize network changes using Infrastructure as Code (IaC)
* Reduce human error in CLI-based operations
* Build reusable automation patterns for enterprise environments
* Integrate validation, reporting, and rollback strategies

---

## 🧠 Core Philosophy

> “If you typed it twice, you should automate it.”

This repository emphasizes:

* **Idempotency** → same result every time
* **Auditability** → every change is tracked
* **Modularity** → reusable roles and tasks
* **Safety** → validate before and after changes

---

## 📁 Repository Structure

```bash
.
├── ansible.cfg
├── inventory/
│   ├── hosts.yml
│   ├── group_vars/
│   └── host_vars/
├── playbooks/
│   ├── show_commands.yml
│   ├── backup_configs.yml
│   ├── deploy_vlan.yml
│   └── validate_state.yml
├── roles/
│   ├── common/
│   ├── vlan/
│   ├── interface/
│   └── backup/
├── templates/
│   └── ios_config.j2
├── files/
├── vars/
└── README.md
```

---

## ⚙️ Requirements

* Python 3.9+
* Ansible 2.14+
* SSH access to Cisco IOS-XE devices
* Network reachability (management VLAN or OOB)

### Install Dependencies

```bash
pip install ansible
ansible-galaxy collection install cisco.ios
```

---

## 🔧 Configuration

### `ansible.cfg`

```ini
[defaults]
inventory = ./inventory/hosts.yml
host_key_checking = False
retry_files_enabled = False
timeout = 30
forks = 10

[privilege_escalation]
become = yes
become_method = enable
```

---

## 🌐 Inventory Example

```yaml
all:
  vars:
    ansible_network_os: ios
    ansible_connection: network_cli
    ansible_user: admin
    ansible_password: password
    ansible_become: true
    ansible_become_method: enable
    ansible_become_password: enable_password

  children:
    core:
      hosts:
        core-sw1:
          ansible_host: 10.0.0.1

    access:
      hosts:
        access-sw1:
          ansible_host: 10.0.1.1
```

---

## ▶️ Running Playbooks

### Test Connectivity

```bash
ansible all -m ping
```

### Run a Playbook

```bash
ansible-playbook playbooks/show_commands.yml
```

### Limit to a Group

```bash
ansible-playbook playbooks/deploy_vlan.yml --limit access
```

---

## 📦 Example Playbook (Show Commands)

```yaml
- name: Gather show commands
  hosts: all
  gather_facts: no

  tasks:
    - name: Run show version
      cisco.ios.ios_command:
        commands:
          - show version
      register: output

    - name: Display output
      debug:
        var: output.stdout_lines
```

---

## 🔐 Security Best Practices

* Use **Ansible Vault** for credentials
* Avoid plaintext passwords in inventory
* Use SSH keys where possible
* Segment management access (OOB or mgmt VLAN)

```bash
ansible-vault encrypt group_vars/all.yml
```

---

## 🧪 Validation & Safety

Before making changes:

* Run in **check mode**

```bash
ansible-playbook playbooks/deploy_vlan.yml --check
```

After changes:

* Validate state with verification playbooks
* Compare running config vs intended state

---

## 🧩 Use Cases in This Repo

* 📡 Device backups (pre/post change)
* 🔁 Config standardization
* 🧱 VLAN deployment at scale
* 🔍 State validation and compliance checks
* ⚡ Rapid incident response changes

---

## 🧱 Next Steps (Roadmap)

* [ ] Integrate with NetBox (source of truth)
* [ ] Add CI/CD pipeline (GitHub Actions)
* [ ] Build compliance dashboards
* [ ] Add RESTCONF / NETCONF examples
* [ ] Implement drift detection

---

## 🤝 Contributing

1. Fork the repo
2. Create a feature branch
3. Submit a pull request

Keep changes modular and documented.


