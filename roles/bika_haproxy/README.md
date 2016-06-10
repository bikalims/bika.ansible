# Bika HAProxy

This role provisions [HAProxy][1] for [Bika LIMS][3].

This role takes care of:

- Installing [HAProxy][1] using the [HAProxy Ansible Role][2]
- Creating a Bika specific configuration

Note: The [HAProxy][1] will listen per default on `127.0.0.1:8080` and takes
care of all specified ZEO clients within the `bika_plone` role.

## Dependencies

This role depends on the [HAProxy Ansible Role][2] and the `bika_plone` role
(within this repository). Therefore, it has to be defined *after* the
`bika_plone` role. See `roles/bika/meta/main.yml` for details.

Note: This role is only used for installation purpose. The full configuration
will be generated within this role and overwrites the generated configuration of
the depending [HAProxy Ansible Role][2].

## Role Variables

Available variables are listed below, along with default values (see
`defaults/main.yml`):




[1]: http://www.haproxy.org "HAProxy"
[2]: https://galaxy.ansible.com/geerlingguy/haproxy "HAProxy Ansible Role"
[3]: https://github.com/bikalabs/bika.lims/wiki "Bika LIMS"
