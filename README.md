nginx
=========

Installs and configures nginx.

Requirements
------------

None.

Role Variables
--------------

```yml
nginx_target:       virtualbox
nginx_home_path:    /vagrant

nginx_app_env:      development
nginx_app_name:     test

nginx_user_name:      '{{ "vagrant" if nginx_target == "virtualbox" else nginx_app_name }}'
nginx_group_name:     '{{ nginx_user_name }}'
nginx_user_home_path: '/home/{{ nginx_user_name }}'

nginx_app_path:         '{{ nginx_home_path if target == "virtualbox" else "{{ nginx_home_path }}/current" }}'
nginx_app_public_path:  '{{ nginx_app_path }}/public'
nginx_app_config_path:  '{{ nginx_app_path }}/config'
nginx_app_temp_path:    '{{ nginx_app_path }}/tmp'
nginx_app_logs_path:    '{{ nginx_app_path }}/log'


nginx_server_type:              nginx_puma
nginx_server_name:              localhost
nginx_ssl_certificate_path:     '/etc/nginx/ssl/{{ nginx_server_name }}.crt'
nginx_ssl_certificate_key_path: '/etc/nginx/ssl/{{ nginx_server_name }}.key'

nginx_puma_bind_path:             'unix://{{ nginx_app_temp_path }}/sockets/puma.{{ nginx_app_env }}.sock'
```

This role is setup to work with any web server you'd want to use with nginx, it is also currently to work with a ruby environment and a php environment. The default is set to use `puma` for a ruby environment. To switch to something else you can change the value of `nginx_server_type` as follows:

```yml
nginx_server_type: nginx_puma         # for Puma (default)
nginx_server_type: nginx_passenger    # for Passenger
nginx_server_type: nginx_unicorn      # for Unicorn
nginx_server_type: nginx_php          # for Php, and specifically WordPress
```

Dependencies
------------

```
bearandgiraffe.base
```

Example Playbook
----------------

```yml
- hosts: servers
  roles:
    - { role: bearandgiraffe.nginx, nginx_server_type: 'puma' }
```

License
-------

The Unlicense (see LICENSE)

Author Information
------------------

Youssef Chaker (@ychaker) from Bear & Giraffe LLC (@bearandgiraffe).
