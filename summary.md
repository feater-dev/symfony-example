# Summary

Symfony is app available at:

* http://localhost:{{{host_port__app_host_network}}}
* http://{{{traefik_port_url__app_traefik_1}}}
* http://{{{traefik_port_url__app_traefik_2}}}

Mailcatcher is available at http://{{{traefik_port_url__mail_traefik}}}

Adminer is available at http://{{{traefik_port_url__adminer_traefik}}}

Database DSN is mysql://{{{env__database_user}}}:{{{env__database_password}}}@symfony_db/{{{env__database_name}}}
