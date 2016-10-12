# Bika Postfix Role

This role ensures a postfix setup for a [Bika LIMS][1] server.


## Dependecies

This role depends on the [Postfix Ansible Role][8].

## Role Variables

Please use the variables from the [Ansible Postfix Role][8].
This role uses some defaults of these variables that listed below, along with
default values (see `vars/main.yml`):

    bika_postfix_smtp_generic_maps:
      - { user: root, alias: root@example.com }

Specify a lookup tables to perform address rewriting in the Postfix
see: http://www.postfix.org/postconf.5.html#smtp_generic_maps

[1]:  https://github.com/bikalabs/bika.lims/wiki "Bika LIMS"
[2]:  https://www.vagrantup.com/docs/getting-started/ "Vagrant"
[3]:  https://www.ansible.com "Ansible"
[4]:  https://docs.ansible.com/ansible/playbooks.html "Ansible Playbook"
[5]:  https://docs.ansible.com/ansible/playbooks_roles.html "Ansible Roles"
[6]:  https://galaxy.ansible.com "Ansible Galaxy"
[7]:  https://docs.ansible.com/ansible/intro_inventory.html "Ansible Inventory"
[8]:  https://galaxy.ansible.com/tersmitten/postfix/ "Postfix Ansible Role"
