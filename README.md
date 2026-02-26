# CML EVPN Lab (Flexible, Variable-Driven)

This starter set provisions a 5-router CML lab:

```
PE1  --  P1  --  P2  --  PE2
     \--  PE4   (PE1 & PE4 all-active multihoming toward CE1)
```

- **Underlay**: ISIS + MPLS LDP on all core-facing links (PEs & Ps)
- **Overlay**: BGP EVPN on PEs (full-mesh iBGP between PE loopbacks)
- **AA Multihoming**: PE1 & PE4 share an ESI and bundle interface
- **Telemetry**: Model-driven telemetry dial-out to your collector (Grafana/Telegraf)

## 1) Prereqs

Create and activate a Python venv, install Ansible, and required collections:

```bash
python3 -m venv venv
source venv/bin/activate
pip install ansible ansible-lint
ansible-galaxy collection install -r collections/requirements.yml -p ./collections
```

## 2) Update device access & interfaces

Edit `group_vars/all.yml` and `host_vars/*`:
- Set `ansible_user` (or in `ansible.cfg` as `remote_user`)
- Adjust `core_ifaces` names per node
- Confirm `loopback_ip` per device
- Set telemetry destination if used

## 3) Run underlay
```bash
ansible-playbook playbooks/underlay.yml
```

## 4) Run overlay (EVPN)
```bash
ansible-playbook playbooks/bgp_evpn.yml
```

## 5) Telemetry (optional)
```bash
ansible-playbook playbooks/telemetry.yml
```

> Tip: The configuration is intentionally minimal and variable-driven. Extend templates to add policies, route-maps, and per-platform specifics as needed.
