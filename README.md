# Ansible Role: apache2

This role installs and configures an Apache2 webserver on Debian Stretch (Debian 9.x).

Example Yaml configuration:

````
apache2_config:
  - file: '/etc/apache2/apache2.conf'
    regex: '^HostnameLookups'
    line: 'HostnameLookups On'
  - file: '/etc/apache2/conf-enabled/security.conf'
    regex: '^ServerTokens'
    line: 'ServerTokens Prod'
  - file: '/etc/apache2/conf-enabled/security.conf'
    regex: '^ServerSignature'
    line: 'ServerSignature Off'

apache2_vhosts:
  - filename: mysite.conf
    servername: www.mysite.tld
    serversignature: 'Off'
    vhost:
      ip: 0.0.0.0
      port: 80
    documentroot: /var/www/wordpress
    redirect:
      - src: '/abc-chat'
        dest: 'https://discord.gg/abc-chat'
    config:
      - 'SetEnvIf Remote_Addr "::1" dontlog'
    directory:
      - name: /var/www/wordpress
        config:
          - "Options Indexes FollowSymLinks MultiViews"
          - "AllowOverride All"
          - "Order allow,deny"
          - "Allow from all"
    errorlog: '/var/log/apache2/mysite.error.log'
    customlog: '"/var/log/apache2/mysite.access.log" combined env=!dontlog'

apache2_vhosts_disable:
  - 000-default.conf
apache2_vhosts_enable:
  - mysite.conf

apache2_modules_disable:
  - mpm_worker.conf
  - mpm_worker.load

````

Example Playbook:

````
- hosts: mysite

  roles:
    - ansible-role-apache2
````

## LICENSE

BSD 2-Clause License

## Author

Stefan Pommerening <pom@dmsp.de>, http://www.dmsp.de