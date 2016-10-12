# Bika MUNIN

This role provisions [MUNIN][1] for [Bika LIMS][3].

This role takes care of:

- Installing [MUNIN][1] using the [MUNIN Ansible Role][2]
- Creating a [Bika LIMS][3] specific configuration

## Dependencies

This role depends on the follwoing Ansible Roles:

- [MUNIN](https://galaxy.ansible.com/geerlingguy/munin)
- [Plone](https://galaxy.ansible.com/plone/plone_server)
- [HAProxy](https://galaxy.ansible.com/geerlingguy/haproxy)
- [Varnish](https://galaxy.ansible.com/geerlingguy/varnish)
- [NGINX](https://galaxy.ansible.com/geerlingguy/nginx)

Note: This role uses the [MUNIN Ansible Role][2] to ensure the installation
      procedure of [MUNIN][1] on the target machnine.

## Role Variables

Available variables are listed below, along with default values (see
`defaults/main.yml`):

    bika_munin_dependencies:
      - libxml-parser-perl

Additional system packages needed for the plugins to run correctly.

    bika_nginx_config_path: /etc/nginx/sites-available/bika.conf

NGINX configuration file location.

    bika_munin_nginx_config: |
      # MUNIN STATIC FILES
      location /munin/static/ {
          alias /etc/munin/static/;
          expires modified +1w;
      }
      # MUNIN INSTANCE (protected by htpasswd)
      location /munin/ {
          auth_basic            "BIKA Munin";
          auth_basic_user_file  /etc/munin/munin-htpasswd;
          alias /var/cache/munin/www/;
          expires modified +310s;
      }
      # NGINX STATUS (needed for the Munin NGINX plugin)
      location /nginx_status {
          stub_status on;
          access_log off;
          allow all;
      }

NGINX configuration to append.

    bika_munin_hostname: "bikalims"

Host name.

    bika_munin_host: 127.0.0.1

Munin listen address.

    bika_munin_port: 4949

Munin listen port.

    bika_munin_allow_ipv4: "^127\\.0\\.0\\.1$"
    bika_munin_allow_ipv6: "^::1$"

Allowed hosts which might query the plugins.

    bika_munin_admin_user: admin
    bika_munin_admin_password: admin

Basic Auth Username/Password to protect the Munin dashboard.

    bika_munin_hosts:
      - {
        name: "BIKA;{{ bika_munin_hostname }}",
        address: "127.0.0.1",
        extra: ["use_node_name yes"]
      }

Munin host `Groupname;Hostname` (see: http://munin-monitoring.org/wiki/use_node_name)

    bika_munin_plugin_conf: |
      [nginx_*]
      user root
      env.url http://127.0.0.1/nginx_status

      [haproxy*]
      user root
      env.backend {{bika_haproxy_backend_id}}
      env.frontend {{bika_haproxy_frontend_id}}
      env.url http://{{bika_haproxy_stats_user}}:{{bika_haproxy_stats_pass}}@{{bika_haproxy_frontend_bind_address}}:{{bika_haproxy_frontend_port}}/haproxy-status;csv;norefresh

      [varnish_*]
      user root
      group varnish
      env.varnishstat varnishstat

      [zeo_monitor_*]
      user root
      env.zeomonitoraddr {{bika_zeo__bind_address}}

      [zope_*]
      user root
      {% for client in range(0, bika_plone_client_count|int) %}
      env.MUNIN_ZOPE_HOST_instance{{client + 1}} http://localhost:{{ bika_plone_client_base_port | int + client }}/munin
      {% endfor %}

Munin Plugin configuration (see: http://munin-monitoring.org/wiki/plugin-conf.d).

    bika_munin_plugins:
      # NGINX PLUGINS
      - src: /usr/share/munin/plugins/nginx_request
        dest: /etc/munin/plugins/nginx_request
      - src: /usr/share/munin/plugins/nginx_status
        dest: /etc/munin/plugins/nginx_status
      # HAPROXY
      - src: /usr/share/munin/plugins/haproxy_ng
        dest: /etc/munin/plugins/haproxy_ng
      # VARNISH PLUGINS
      - src: /etc/munin/contrib/plugins/varnish4/varnish4_
        dest: /etc/munin/plugins/varnish4_backend_traffic
      - src: /etc/munin/contrib/plugins/varnish4/varnish4_
        dest: /etc/munin/plugins/varnish4_bad
      - src: /etc/munin/contrib/plugins/varnish4/varnish4_
        dest: /etc/munin/plugins/varnish4_expunge
      - src: /etc/munin/contrib/plugins/varnish4/varnish4_
        dest: /etc/munin/plugins/varnish4_hit_rate
      - src: /etc/munin/contrib/plugins/varnish4/varnish4_
        dest: /etc/munin/plugins/varnish4_memory_usage
      - src: /etc/munin/contrib/plugins/varnish4/varnish4_
        dest: /etc/munin/plugins/varnish4_objects
      - src: /etc/munin/contrib/plugins/varnish4/varnish4_
        dest: /etc/munin/plugins/varnish4_request_rate
      - src: /etc/munin/contrib/plugins/varnish4/varnish4_
        dest: /etc/munin/plugins/varnish4_threads
      - src: /etc/munin/contrib/plugins/varnish4/varnish4_
        dest: /etc/munin/plugins/varnish4_transfer_rates
      - src: /etc/munin/contrib/plugins/varnish4/varnish4_
        dest: /etc/munin/plugins/varnish4_uptime

Activated (symlinked) plugins.

This role integrates into the `bika_control_panel: http://<server-ip>/bika_control_panel`.


[1]: http://munin-monitoring.org "MUNIN"
[2]: https://galaxy.ansible.com/geerlingguy/munin "MUNIN Ansible Role"
[3]: https://github.com/bikalabs/bika.lims/wiki "Bika LIMS"
