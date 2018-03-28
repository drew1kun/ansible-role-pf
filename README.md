Ansible role: pf
=========

[![MIT licensed][mit-badge]][mit-link]
[![Galaxy Role][role-badge]][galaxy-link]

Ansible role for hardening configuration of pf firewall for MacOS (BSD will come soon)

# PF Anchors
The role will create the following pf anchors:
 - sshd anchor (drew.sshd.rules) with bruteforce protection table and rules
 - emerging-treats open ruleset anchor (drew.eto.rules) with table of known attackers' IPs and rules for blocking them
 - custom anchor with rules specified in defaults/main.yml

Those anchors will be loaded in main pf configuration file

**NOTE:**
The system default /etc/pf.conf on MacOS can be overwritten during system updates, therefore custom /etc/pf.drew.conf
will be created and /etc/pf.conf will be included in that custom config.

# Launchd jobs
On MacOS the following Launchd jobs will be created:
 - job for clearing the ssh_bruteforce table once a day at 10:00pm
 - job for automated download of ETOpen ruleset once a day at 10:05pm

Requirements
------------

On MacOS 10.11+ (EI Captain) the System Integrity Protection must be disabled in order to be able to modify /System/
files.

**Reminder:** Ansible uses ssh so make sure the pf configuration does not block ssh

Role Variables
--------------
OS-Agnostic:

| Variables | Description | Default|
|-----------|-------------|--------|
| **pf_macros** | Macros variables for pf configuration | see [`defaults/main.yml`](defaults/main.yml) |
| **pf_tables** | pf configuration tables | see [`defaults/main.yml`](defaults/main.yml) |
| **pf_options** | pf configuration options | see [`defaults/main.yml`](defaults/main.yml) |
| **pf_normalization** | pf configuration normalization rules | see [`defaults/main.yml`](defaults/main.yml) |
| **pf_queueing** | pf configuration queueing | see [`defaults/main.yml`](defaults/main.yml) |
| **pf_translation** | Redirect rules for port forwarding and NAT | see [`defaults/main.yml`](defaults/main.yml) |
| **pf_filtering** | Stateful and stateless filtering for rule-based packet blocking or passing | see [`defaults/main.yml`](defaults/main.yml) |

OS-Specific:

| Variables | Description | Default|
|-----------|-------------|--------|
| **pf_config** | path to main pf configuration file | <ul><li>Darwin: `/etc/pf.drew.conf`</li><li>Others (BSD): `/etc/pf.conf`</li></ul> |

Dependencies
------------

None

Example Playbook
----------------

    - hosts: dev_clients_macos
      gather_facts: yes
      roles:
        - drew-kun.pf

License
-------

[MIT][mit-link]

Author Information
------------------

Andrew Shagayev | [e-mail](mailto:drewshg@gmail.com)

[role-badge]: https://img.shields.io/badge/role-drew--kun.pf-green.svg
[galaxy-link]: https://galaxy.ansible.com/drew-kun/pf/
[mit-badge]: https://img.shields.io/badge/license-MIT-blue.svg
[mit-link]: https://raw.githubusercontent.com/drew-kun/ansible-pf/master/LICENSE
