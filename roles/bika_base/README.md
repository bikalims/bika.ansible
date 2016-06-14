# Bika Base Role #

This role provisions all requirements for a system to run a [Bika LIMS][1]
server.

This role takes care of:

- Updating the Distribution
- Installing the required OS packages for [Bika LIMS][1]
- Configure APT settings


## Dependencies ##

None


## Role Variables ##

Available variables are listed below, along with default values (see
`defaults/main.yml`):

    bika_apt_update_package_lists: 1

Frequency (in days) at which the package lists are refreshed

    bika_apt_download_upgradable_packages: 1

Frequency (in days) for the downloading of the actual packages.

    bika_apt_autocleaninterval: 7

Frequency (in days) how often obsolete packages (those not referenced by any
distribution anymore) are removed from the APT cache.

    bika_apt_unattended_upgrade: 1

When this option is enabled, the daily script will execute unattended-upgrade


### Ubuntu ###

This role includes variables depending on the `ansible_distribution`.
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
        - acl

The variable `bika_packages` lists the required packages which get installed.

[1]: https://github.com/bikalabs/bika.lims/wiki "Bika LIMS"
[2]: https://plone.org "Plone"
