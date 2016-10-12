# Bika Meta Package

This role provisions a complete [Bika LIMS][1] installation, containing the
following components:

- Bika Base Setup
- Bika Plone Installation
- Bika HAProxy (Load Balancer)
- Bika Varnish (Cache Proxy)
- Bika NGINX (Webserver)
- Bika Security (Firewall/Fail2Ban)
- Bika Munin (Monitoring)
- Bika Postfix (Mail)

> **Note:**
> Ansible Version > 2 is required

## Dependencies

This role depends on the follwoing Ansible Roles:

- [Plone](https://galaxy.ansible.com/plone/plone_server)
- [HAProxy](https://galaxy.ansible.com/geerlingguy/haproxy)
- [Varnish](https://galaxy.ansible.com/geerlingguy/varnish)
- [NGINX](https://galaxy.ansible.com/geerlingguy/nginx)
- [Firewall](https://galaxy.ansible.com/HanXHX/firewall)
- [Munin](https://galaxy.ansible.com/geerlingguy/munin)
- [Postfix](https://galaxy.ansible.com/tersmitten/postfix)

## Installation

You can install the dependencies using `ansible_galaxy` pointing to the
`requirements.yml` file within this repository.
The roles will be installed to the `roles_path` defined in `ansible.cfg` (also
in this repository), which will use the local `roles` folder.

    ansible-galaxy install -r requirements.yml


## Playbook

Within a [Playbook](http://docs.ansible.com/ansible/playbooks.html) you just
need to include this role for a **full monty** installation.

    ---
    - hosts: bika
      become: yes
      gather_facts: yes
      pre_tasks:
        - name: Update APT package cache
          action: apt update_cache=yes
        - name: Upgrade APT to the lastest packages
          action: apt upgrade=full
        - name: Include Custom Variables `custom_configure.yml`
          include_vars: custom_configure.yml

      roles:
        - role: bika

Note: You can override variables within your `custom_configure.yml`, e.g.

    ---
    bika_plone_initial_password: s3cr3t

[1]: https://github.com/bikalabs/bika.lims/wiki "Bika LIMS"
