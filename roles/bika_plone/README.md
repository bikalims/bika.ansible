# Bika Plone Role

This role provisions a [Bika LIMS][1] installation running on a suitable
[Plone][2] server.

This role takes care of:

- Installing the required OS packages
- Creating a dedicated `bika` system user and group
- Installing [Plone][2] in the right version
- Building a ZEO cluster
- Installing `bika.lims` and `bika.health`

## Dependecies

This role depends on the [Plone Server Role][4].

## Role Variables

Available variables are listed below, along with default values (see
`defaults/main.yml`):

    bika_plone_config:
      - plone_version: "{{ bika_plone_version }}"
        plone_group: "{{ bika_group }}"
        plone_buildout_user: "{{ bika_user }}"
        plone_daemon_user: "{{ bika_daemon_user }}"
        plone_initial_password: "{{ bika_plone_initial_password }}"
        plone_instance_name: "{{ bika_plone_instance_name }}"
        plone_create_site: "{{ bika_plone_create_site }}"
        plone_target_path: "{{ bika_plone_target_path }}"
        plone_var_path: "{{ bika_plone_var_path }}"
        plone_additional_eggs: "{{ bika_additional_eggs }}"
        plone_additional_versions: "{{ bika_additional_versions }}"
        plone_client_count: "{{ bika_plone_client_count }}"
        plone_zserver_threads: "{{ bika_plone_zserver_threads }}"
        plone_zeo_ip: "{{ bika_plone_zeo_ip }}"
        plone_zeo_port: "{{ bika_plone_zeo_port }}"
        plone_client_base_port: "{{ bika_plone_client_base_port }}"

Bika specific configuration for [Plone][2].

You can overwrite this variable to your needs using the variables defined in the
[Plone Server Role][4], for example like this:

    bika_plone_config:
      - plone_version: "4.3.8"
        plone_initial_password: admin
        plone_additional_eggs:
          - bika.lims
          - bika.health
        plone_additional_versions:
          - bika.lims=3.1.9
          - bika.health=3.1.8

Please note, that there is only **one** item supported below `bika_plone_config`
(Note the `-` below the variable).


## Convenience Variables

This role defines some convenience variables to easier overwrite some common
settings within the [Plone Server Role][4].

Note: Besides the `bika_user` variable, all listed variables are proxies of the
      defined variables within the [Plone Server Role][4] prefixed with `bika_`.

    bika_user: bika

The user which "owns" the buildout and where everything gets installed.

    bika_group: bika

The default group `bika_user` belongs to.

    bika_daemon_user: bika_daemon

The user who runs the process of the server.

    bika_plone_version: "4.3.8"

The [Plone][2] version to be used – Bika currently only the 4.x series.

    bika_plone_initial_password: admin

The "admin" user password – please overwrite for productive instances.

    bika_plone_instance_name: bikalims

Sets the name that discriminates this install from others. This name should be
globally unique on the server as it's used to discriminate between supervisor
and cron jobs. It is also the name of the folder where Bika gets installed, e.g.
`/home/bika/bikalims`.

    bika_plone_site_id: bikalims

The id of the site if `bika_plone_site` is set to `yes`

    bika_plone_create_site: no

Set to `yes` to create a Plone site automatially.

    bika_plone_additional_eggs:
      - bika.lims

List additional Python packages (beyond Plone and the Python Imaging Library)
that you want available in the Python package environment.

    bika_plone_additional_versions:
      - bika.lims=3.1.9

The version pins you specify here will be added to the [versions] section of
your Bika buildout.

    bika_plone_target_path: /home/{{ bika_user }}

Sets the Bika installation directory.

    bika_plone_var_path: /home/{{ bika_user }}/data

The `var` directory to use. This is where the `filestorage` and `blobstorage` is
located.

    bika_plone_client_count: 2

Number of Bika ZEO clients.

    bika_plone_zserver_threads: 2

Number of threads per Bika Instance.

    bika_plone_zeo_ip: 127.0.0.1

The IP of the ZEO Server.

    bika_plone_zeo_port: 8100

The port where the ZEO Server listens for connections.

    bika_plone_client_base_port: 8081

The base port for the ZEO clients. Per default, two ZEO clients will be
installed, with the follwoing addresses: `127.0.0.1:8081` and `127.0.0.1:8082`.

[1]: https://github.com/bikalabs/bika.lims/wiki "Bika LIMS"
[2]: https://plone.org "Plone"
[3]: https://galaxy.ansible.com "Ansible Galaxy"
[4]: https://github.com/plone/ansible.plone_server "Plone Server Role"
[5]: https://galaxy.ansible.com/plone/plone_server "Plone Server on Galaxy"
