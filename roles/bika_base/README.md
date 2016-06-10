# Bika Base Role

This role provisions all requirements for a system to run a [Bika LIMS][1]
server.

This role takes care of:

- Installing the required OS packages for [Bika LIMS][1]

## Dependencies

None

## Role Variables

This role includes variables depending on the `ansible_distribution`.

### Ubuntu

Required packages for Ubuntu can be found in `vars/Ubuntu.yml`

    bika_packages:
        - git
        - htop
        - screen
        - tree
        - lynx
        - libffi-dev
        - gcc
        - zlib1g-dev
        - libexpat1-dev
        - libxslt1.1
        - gnuplot
        - libcairo2
        - libpango1.0-0
        - libgdk-pixbuf2.0-0

The variable `bika_packages` lists the required packages which get installed.

[1]: https://github.com/bikalabs/bika.lims/wiki "Bika LIMS"
[2]: https://plone.org "Plone"
