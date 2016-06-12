# Bika NGINX

This role provisions [NGINX][1] for [Bika LIMS][3].

This role takes care of:

- Installing [NGINX][1] using the [NGINX Ansible Role][2]
- Creating a [Bika LIMS][3] specific configuration

## Dependencies

This role depends on the follwoing Ansible Roles:

- [NGINX](https://galaxy.ansible.com/geerlingguy/nginx)

Note: This role uses the [NGINX Ansible Role][2] to ensure the installation
      procedure of [NGINX][1] on the target machnine.

## Role Variables

Available variables are listed below, along with default values (see
`defaults/main.yml`):

    bika_nginx_path: "/etc/nginx"

The path to the NGINX installation – currently only supports Ubuntu machines.

    bika_nginx_html_root: "/usr/share/nginx/bika"

The root path for static HTML files – currently only supports Ubuntu machines.

    bika_nginx_sites_available: "{{ bika_nginx_path }}/sites-available"

The folder where all virtual host files get stored – currently only supports
Ubuntu machines.

    bika_nginx_sites_enabled: "{{ bika_nginx_path }}/sites-enabled"

The folder where active virtual host files get symlinked – currently only supports
Ubuntu machines

Note: All files within `bika_nginx_sites_available` which start with `bika` get
symlinked by this role per default.

    bika_nginx_conf: "{{ bika_nginx_path }}/nginx.conf"

The NGINX main configuration file - currently only supports Ubuntu machines.

    bika_nginx_extra_http_options: |
      proxy_read_timeout 600s;

NGINX extra configuration parameteres, which get written to the `http` section of NGINX.


## Bika Configuration

This role creates a nginx configuration (`/etc/nginx/nginx.conf`) with some sane
defaults (timeouts, size-limits, upstreams etc.). See the file
`templates/nginx.conf.j2` and `defaults/main.yml` for details about the
configuration options.

    server_names_hash_bucket_size 256;
    client_max_body_size 1024m;
    proxy_read_timeout 600s;

## Special Routes

This role generates some special routes:

    - Bika Control Panel: `http://<server-ip>/bika_control_panel`
    - HAProxy Status: `http://<server-ip>/haproxy-status`

[1]: http://www.nginx.org "NGINX"
[2]: https://galaxy.ansible.com/geerlingguy/nginx "NGINX Ansible Role"
[3]: https://github.com/bikalabs/bika.lims/wiki "Bika LIMS"
