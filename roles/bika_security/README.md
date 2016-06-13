# Bika Plone Role

This role ensures a secure environment for [Bika LIMS][1].


## Dependecies

This role depends on the [Firewall Role][2].

## Role Variables

Available variables are listed below, along with default values (see
`defaults/main.yml`):

    bika_firewall_open_tcp_ports: [80, 443]

Input TCP open ports list.

Note: SSH Port is dynamically fetched and open by default.




[1]: https://github.com/bikalabs/bika.lims/wiki "Bika LIMS"
[2]: https://galaxy.ansible.com/HanXHX/firewall/ "Firewall"
