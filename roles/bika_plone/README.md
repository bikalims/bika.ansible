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
        plone_backup_path: "{{ bika_plone_backup_path }}"
        plone_backup_at: "{{ bika_plone_backup_at }}"
        plone_keep_backups: "{{ bika_plone_keep_backups }}"
        plone_keep_blob_days: "{{ bika_plone_keep_blob_days }}"
        plone_pack_at:  "{{ bika_plone_pack_at }}"
        plone_keep_days: "{{ bika_plone_keep_days }}"
        plone_rsync_backup_options:  "{{ bika_plone_rsync_backup_options }}"
        plone_additional_eggs: "{{ bika_additional_eggs }}"
        plone_zcml_slugs: "{{ bika_plone_zcml_slugs + bika_monitoring_zcml }}"
        plone_additional_versions: "{{ bika_additional_versions }}"
        plone_client_count: "{{ bika_plone_client_count }}"
        plone_zserver_threads: "{{ bika_plone_zserver_threads }}"
        plone_zeo_ip: "{{ bika_plone_zeo_ip }}"
        plone_zeo_port: "{{ bika_plone_zeo_port }}"
        plone_client_base_port: "{{ bika_plone_client_base_port }}"
        plone_buildout_extra: "{{ bika_plone_buildout_extra }}"
        plone_sources: "{{ bika_plone_sources }}"

Bika specific configuration for [Plone][2].

You can overwrite this variable to your needs using the variables defined in the
[Plone Server Role][4], for example like this:

    bika_plone_config:
      - plone_version: "4.3.11"
        plone_initial_password: admin
        plone_additional_eggs:
          - bika.lims
        plone_additional_versions:
          - bika.lims=3.1.13

Please note, that there is only **one** item supported below `bika_plone_config`
(Note the `-` below the variable).


## Convenience Variables

This role defines some convenience variables to easier overwrite some common
settings within the [Plone Server Role][4].

Note: Besides the `bika_user` variable, all listed variables are proxies of the
      defined variables within the [Plone Server Role][4] prefixed with `bika_`.


### System Users

    bika_user: bika

The user which "owns" the buildout and where everything gets installed.

    bika_group: bika

The default group `bika_user` belongs to.

    bika_daemon_user: bika_daemon

The user who runs the process of the server.


### Bika LIMS

    bika_plone_version: "4.3.11"

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

    bika_plone_create_site: yes

Set to `yes` to create a Plone site automatially.

    bika_plone_additional_eggs:
      - bika.lims

List additional Python packages (beyond Plone and the Python Imaging Library)
that you want available in the Python package environment.

    bika_plone_additional_versions:
      - bika.lims=3.1.13

The version pins you specify here will be added to the [versions] section of
your Bika buildout.

    bika_plone_zcml_slugs:
        - plone.reload

List additional ZCML slugs that may be required by older packages that don't
implement auto-discovery. The default list is empty. This is rarely needed.

    bika_plone_extension_profiles: []

List additional Plone profiles which should be activated in the new Plone site.
These are only activated if the `bika_plone_create_site` is set.

    bika_plone_buildout_extra: |
      allow-picked-versions = false
      socket-timeout = 5

Allows you to add settings to the automatically generated buildout. Any text
specified this way is inserted at the end of the ``[buildout]`` part and before
any of the other parts. Defaults to empty.

    bika_plone_sources =
      -  "my.package = svn http://example.com/svn/my.package/trunk update=true"
      -  "some.other.package = git git://example.com/git/some.other.package.git rev=1.1.5"

This setting allows you to check out and include repository-based sources in
your buildout.

Source specifications, a list of strings
in [mr.developer](https://pypi.python.org/pypi/mr.developer) sources format. If
you specify plone_sources, the mr.developer extension will be used with
auto-checkout set to "*".


### Installation Paths

    bika_plone_target_path: /home/{{ bika_user }}

Sets the Bika installation directory.

    bika_plone_var_path: /home/{{ bika_user }}/data

The `var` directory to use. This is where the `filestorage` and `blobstorage` is
located.


### Backups

The following `backup` and `pack` cronjobs are added for the `bika_daemon` user.
To see them, you have to type `sudo crontab -u bika_daemon -l`. Please ensure
that the `bika_plone_backup_path` is accessible for this user, otherwise this
won't work.

    bika_plone_backup_path: /home/{{ bika_user }}/backup

The `backup` directory to use.

    bika_plone_backup_at:
      minute: 30
      hour: 2
      weekday: "*"

When do you wish to run the backup operation? Specify minute, hour and weekday
specifications for a valid *cron* time. See `CRONTAB(5)`. Defaults to 2:30 every
morning. Set to `no` to avoid creation of a cron job.

NOTE: The `backup` and `pack` cronjobs are added for the `bika_daemon` user. To
      see them, you have to type `sudo crontab -u bika_daemon -l`. Please ensure
      that the `bika_plone_backup_path` is accessible for this user, otherwise
      this won't work.

    bika_plone_keep_backups: 5

How many generations of full backups do you wish to keep? Defaults to `5`.

    bika_plone_keep_blob_days: 21

How many days of blob backups do you wish to keep? This is typically set to
keep_backups * days_between_packs days. Default is `21`.

    bika_plone_pack_at:
      minute: 30
      hour: 1
      weekday: 7

When do you wish to run the ZEO pack operation? Specify minute, hour and weekday
specifications for a valid cron time. See CRONTAB(5). Defaults to 1:30 Sunday
morning. Set to no to avoid creation of a cron job.

    bika_plone_keep_days: 3

How many days of undo information do you wish to keep when you pack the database. Defaults to `3`.


### ZEO Cluster Setup

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

    bika_plone_zodb_cache_size: 30000

Defines the objects to keep in the ZODB cache.

Note: A higher value consumes more RAM


### Supervisor HTTP

This role can enable or disable the HTTP UI of supervisor, which is accessible
on `http://<server-ip>/supervisor/`.

Important: It is possible to start/stop the Instances through this UI, so handle
           with care and set a strong password.

    bika_supervisor_with_http: no

Enable/Disable HTTP UI of Supervisor.

    bika_supervisor_http_port: "127.0.0.1:9001"

The listen address and port of the HTTP UI.
Note: The `bika_nginx` role will make this UI accessible through
      `http://<server-ip>/haproxy/`

    bika_supervisor_http_user: "admin"

HTTP Basic-Auth username for the Supervisor HTTP UI.

    bika_supervisor_http_pass: "admin"

HTTP Basic-Auth password for the Supervisor HTTP UI.


[1]: https://github.com/bikalabs/bika.lims/wiki "Bika LIMS"
[2]: https://plone.org "Plone"
[3]: https://galaxy.ansible.com "Ansible Galaxy"
[4]: https://github.com/plone/ansible.plone_server "Plone Server Role"
[5]: https://galaxy.ansible.com/plone/plone_server "Plone Server on Galaxy"
