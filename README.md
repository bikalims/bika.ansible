# Bika Ansible

[![Build Status](https://travis-ci.org/bikalabs/bika.ansible.svg?branch=master)](https://travis-ci.org/bikalabs/bika.ansible)

[Bika Ansible][6] provides a set of [Ansible Roles][7] to set up a
[Bika LIMS][1] installation on a Linux server.

Each role contains a `README.md` with further documentation.

> **Note:**
> Ansible Version >= 2 is required

## Getting started

Install dependencies using `ansible-galaxy`

    ansible-galaxy install -r requirements.yml

Create an [Ansible Playbook][9], e.g. name it `bika.yml`:

    ---
    - hosts: all
      become: yes
      gather_facts: yes
      pre_tasks:
        - name: Update APT package cache
          action: apt update_cache=yes
      roles:
        - role: bika

Create an [Ansible Inventory][8] file, e.g. name it `bika_hosts.cfg`:

    [bika]
    bika ansible_ssh_host=192.168.33.10 ansible_ssh_user=vagrant ansible_ssh_private_key_file=.vagrant/machines/app/virtualbox/private_key

Run your [Ansible Playbook][9]:

    ansible-playbook -i bika_hosts.cfg bika.yml

Please refer to the `vagrant.yml` [Ansible Playbook][9] for a working example.


## Vagrant

This repository contains a running [Vagrant][10] setup, which creates a full
[Bika LIMS][1] installation, containing the following components:

- Bika Base Setup
- Bika Plone Installation
- Bika HAProxy (Load Balancer)
- Bika Varnish (Cache Proxy)
- Bika NGINX (Webserver)

### Vagrant Setup

Start the [Vagrant][10] machine:

    vagrant up

This will download a Linux machine according to the `Vagrantfile` within this
repository. The server will listen on `192.168.33.10`.

You can access it via SSH:

    vagrant ssh

As soon as the machine is running, you can start the provisioning process:

    ansible-playbook -i vagrant_hosts.cfg vagrant.yml

> If you are using Vagrant < 1.7, you should read vagrant_hosts.cfg first,
> and uncomment the line with the correct private_key file path, or ansible
> will not be able to connect.

Open a browser and navigate to: http://192.168.33.10


## Dependencies

This role depends on the following Ansible Roles:

- [Plone](https://galaxy.ansible.com/plone/plone_server)
- [HAProxy](https://galaxy.ansible.com/geerlingguy/haproxy)
- [Varnish](https://galaxy.ansible.com/geerlingguy/varnish)
- [NGINX](https://galaxy.ansible.com/geerlingguy/nginx)
- [Firewall](https://galaxy.ansible.com/HanXHX/firewall)



[1]: https://github.com/bikalabs/bika.lims/wiki "Bika LIMS"
[2]: https://plone.org "Plone"
[3]: https://galaxy.ansible.com "Ansible Galaxy"
[4]: https://github.com/plone/ansible.plone_server "Plone Server Role"
[5]: https://galaxy.ansible.com/plone/plone_server "Plone Server on Galaxy"
[6]: https://github.com/bikalabs/bika.ansible "Bika Ansible"
[7]: https://docs.ansible.com/ansible/playbooks_roles.html "Ansible Roles"
[8]: https://docs.ansible.com/ansible/intro_inventory.html "Ansible Inventory"
[9]: https://docs.ansible.com/ansible/playbooks.html "Ansible Playbooks"
[10]: https://www.vagrantup.com/docs/getting-started/ "Vagrant"
