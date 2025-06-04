# 🖥️ Clustering using Ansible

Ansible automation to build a High Availability (HA) Cluster using **Pacemaker**, **Corosync**, and **DRBD** on Ubuntu 24.04.

## 📦 Components

- Pacemaker & Corosync for cluster resource management.
- DRBD for block-level data replication.
- Apache as a sample web service.
- Cluster controlled via Ansible roles.

## 🛠️ Requirements

- 2 Ubuntu 24.04 servers (tested on `node-ha-1` and `node-ha-2`)
- Passwordless SSH access between Ansible host and target nodes
- Python 3, Ansible installed on control node
- VirtualBox or any virtualization tool (for testing)


## 🚀 How to Use

### 1. Clone the repo

```bash
git clone https://github.com/Yasmine-Mamdouh/Clustering-using-Ansible.git
cd Clustering-using-Ansible
```

### 2. Edit inventory

```ini
[cluster_nodes]
node-ha-1 ansible_connection=local ansible_host=node-1-ip hostname=node-ha-1
node-ha-2 ansible_host=node-2-ip hostname=node-ha-2

[cluster_nodes:vars]
ansible_user=user
```

### 3. Run the main playbook

```bash
ansible-playbook -i inventory.ini playbook.yml
```

### 4. Validate

- Check cluster status:

```bash
pcs status
```

- DRBD status:

```bash
cat /proc/drbd
```

- Apache test:

```bash
curl http://<Cluster-IP>
```

## ⚠️ Notes

- DRBD devices need dedicated LVM volumes.
- IPs in DRBD config **must exist on the interface** before `drbdadm up`.
- You might need to reboot nodes manually once during setup if NetworkManager isn’t managing interfaces correctly.

## 🧯 Troubleshooting

- If DRBD fails to start:
  - Check for duplicate `meta-disk` usage.
  - Run `drbdadm down <res>`, fix config, then `drbdadm create-md <res>`.
- Use `journalctl -xeu drbd.service` for deeper errors.

## 📚 Reference

This project follows the official guide:

- [Clusters from Scratch — Pacemaker/Corosync/DRBD Documentation](https://clusterlabs.org/projects/pacemaker/doc/deprecated/en-US/Pacemaker/1.1/html/Clusters_from_Scratch/index.html) from ClusterLabs.


---
