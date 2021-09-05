OpenWRT Config System
=========
Ansible role that configures basic system settings of OpenWRT device (mainly those which are usually defined in `/etc/config/system`).

Requirements
------------
None

Role variables
--------------
Configuration settings defined in `openwrt_config_system` variable which are to be applied on OpenWRT device.
```yaml
openwrt_config_system:
  hostname: myrouter
  timezone: "CET-1CEST,M3.5.0,M10.5.0/3"
  zonename: "Europe/Warsaw"
  conloglevel: 8
  cronloglevel: 6
  log_ip: 127.0.0.1
  log_proto: udp
  ntp:
    enabled: 1
    enable_server: 0
```
If `openwrt_config_system` configuration variable is not present - no changes are made by role.

Dependencies
------------
This role depends on Ansible role `gekmihesg.openwrt` which allows to manage OpenWRT and derivatives with Ansible but without Python.

Example playbook using the role
-------------------------------
`./host_vars` \
`./host_vars/myrouter.yml`
```yaml
---
openwrt_config_system:
  hostname: myrouter
  timezone: "CET-1CEST,M3.5.0,M10.5.0/3"
  zonename: "Europe/Warsaw"
```
`./inventory.ini`
```ini
[openwrt]
myrouter
```
`./roles` \
`./roles/requirements.yml`
```yaml
---
roles:
- name: ansible_role_openwrt
  src: gekmihesg.openwrt
- name: ansible_role_openwrt_config_system
  src: pe_pe.openwrt_config_system
```
`./site.yml`
```yaml
---
- hosts: all
  roles:
    - role: ansible_role_openwrt
    - role: ansible_role_openwrt_config_system
```
`./ansible.cfg`
```ini
[defaults]
# Define inventory location
inventory = ./inventory.ini
# Where to put roles downloaded from galaxy and other repos
roles_path = ./roles
# Defaults to /tmp to avoid flash wear on target device
remote_tmp = /tmp
```

Example execution
-----------------
Install roles and requirements:
```
ansible-galaxy role install -r roles/requirements.yml
```
Preview changes execution would perform on inventory:
```
ansible-playbook site.yml --check --diff
```
Execute playbook on inventory:
```
ansible-playbook site.yml
```
License
-------
MIT

Author Information
------------------
PePe