k3s
=========

Registering a small k3s cluster with HA etcd. Adapted from: https://github.com/k3s-io/k3s-ansible

Requirements
------------

This assumes two inventory groups:
1. [k3servers] - Minimum three (3) systems for high availabilty
2. [k3agents] - Minimum two (2) systems

I have a passworded sudo, so I also include var files to enable the 'become' action.

The sample playbook will use the first server in the servers group to initialize the cluster.

Role Variables
--------------

```
systemd_dir: /etc/systemd/system
k3s_master_ip: "{{ hostvars[groups['k3servers'][0]]['ansible_host'] | default(groups['k3servers'][0]) }}"
k3s_extra_server_args: ""
k3s_extra_agent_args: ""
k3s_version: v1.27.1+k3s1
```

Dependencies
------------



Example Playbook
----------------

```
---

# Download configuration information and prepare IP forwarding where applicable.
- hosts: k3servers:k3agents
  gather_facts: yes
  become: yes
  vars_files:
    - ./vars/vars.yml
    - ./vars/vault.yml
  roles:
    - role: k3s/prereq
    - role: k3s/download

# Designating the first host in the server list as the primary node to initialize the control-plane cluster.
- hosts: k3servers[0]
  become: yes
  vars_files:
    - ./vars/vars.yml
    - ./vars/vault.yml
  roles:
    - role: k3s/server-primary

# Prepare other control-plane servers and join them to the cluster.
- hosts: k3servers[1:]
  become: yes
  vars_files:
    - ./vars/vars.yml
    - ./vars/vault.yml
  roles:
    - role: k3s/server-members

# Prepare agent nodes and add to the cluster.
- hosts: k3agents
  become: yes
  vars_files:
    - ./vars/vars.yml
    - ./vars/vault.yml
  roles:
    - role: k3s/agent
```

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
