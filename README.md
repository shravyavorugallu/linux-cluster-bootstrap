# linux-cluster-bootstrap

Ansible playbooks for provisioning and configuring RHEL/CentOS compute clusters from bare-metal to production-ready. Designed for HPC environments with shared storage, centralized auth, and observability.

## What it does

| Role | What gets configured |
|---|---|
| `common` | Hostname, baseline packages, chrony NTP, sysctl tuning, tuned profile |
| `ssh_hardening` | PermitRootLogin, PasswordAuth, MaxAuthTries, AllowGroups, X11 |
| `nfs_client` | NFS4 mounts for `/home`, `/scratch`, `/apps` via fstab |
| `ldap_client` | SSSD + LDAP auth, auto-homedir creation via oddjobd |
| `monitoring` | node_exporter (Prometheus) with systemd + process collectors |

After baseline, head nodes get `slurmctld` and compute nodes get `slurmd`.

## Quick start

```bash
# Install Ansible collections
ansible-galaxy collection install -r requirements.yml

# Dry run
ansible-playbook playbooks/site.yml --check --diff

# Full deploy
ansible-playbook playbooks/site.yml
```

## Inventory

Edit `inventory/cluster.ini` and `group_vars/all.yml` for your environment:

```ini
[head_nodes]
head01 ansible_host=10.0.0.10

[compute_nodes]
node[01:08] ansible_host=10.0.0.[11:18]
```

## Tested on
- RHEL 9 / CentOS Stream 9
- Ansible 2.14+

## Tech
`Ansible` · `RHEL/CentOS` · `NFS` · `LDAP/SSSD` · `SLURM` · `Prometheus node_exporter` · `sysctl` · `chrony`
