ssh-fingerprinter
=========

For newly deployed machines, this collects ssh finger prints to avoid having to do this step when running another playbook, especially if there are a larger number of hosts to process.

Requirements
------------

Ansible inventory file with available ansible_host values.

Optional - Can be modified to use inventory_hostname instead if you have DNS configured.

Role Variables
--------------

None.

Dependencies
------------

None.

Example Playbook
----------------

```
# Collect SSH fingerprints from known hosts prior to running any other Ansible play.
---
  - hosts: all
    gather_facts: False
    roles:
      - ssh-fingerprinter
```

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).