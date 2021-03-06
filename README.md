# Ansible Sentry

Ansible role which install and setup [Sentry](https://getsentry.com) 8.


### Requirements

- Stouts.nginx
- Stouts.python
- Stouts.supervisor
- ANXS.postgresql
- DavidWittman.redis


#### Variables

The role variables and default values.

```yaml
sentry_enabled: yes                                       # Enable the role
sentry_cron: no
sentry_remove: no                                         # Uninstall the role
sentry_version: 8.0.2
sentry_home: /usr/lib/sentry                              # Deploy sentry to the folder
sentry_user: sentry                                       # Run as user
sentry_hostname: "{{inventory_hostname}}"
sentry_port: 80
sentry_url_prefix: "http://{{sentry_hostname}}"
sentry_secret_key: 1LsmGR1DIyCJ5n2bRG5IVOFHdzEPkTKlW0RzxZVe9S0vc
sentry_extensions: []                                     # List of sentry-extensions

# Python configuration
sentry_python: python2.7                                  # In the case of multiple Python  installations
                                                          # Pick one for Sentry using specific virtualenv command

sentry_ssl_certificate:                                   # SSL certificate file - also turns on HTTPS on Nginx
sentry_ssl_certificate_key:                               # Key file for SSL cert

sentry_config_additional: []                              # List of additional options

# Setup gunicorn
sentry_web_host: 127.0.0.1
sentry_web_port: 9000
sentry_web_options: { workers: 3, limit_request_line: 0, secure_scheme_headers: {'X-FORWARDED-PROTO': 'https'} }

# Setup databases
sentry_db_name: sentry
sentry_db_user: postgres
sentry_db_password: ""
sentry_db_host: ""
sentry_db_port: ""
sentry_db_options: {}

sentry_redis_host: "{{redis_bind}}"
sentry_redis_port: "{{redis_port}}"
sentry_broker_url: "redis://{{redis_bind}}:{{redis_port}}"

# Email settings
sentry_server_email: 'sentry@{{sentry_hostname}}'         # From email

sentry_email_host: 'localhost'
sentry_email_password: ''
sentry_email_user: 'sentry'
sentry_email_port: 25
sentry_email_use_tls: True
sentry_email_use_ssl: False

sentry_single_organization: True
sentry_filestore: 'django.core.files.storage.FileSystemStorage'
sentry_filestore_options:
  location: '/tmp/sentry-files'
sentry_allowed_hosts: ['*']

# Logging
sentry_access_log: /var/log/sentry-access.log
sentry_error_log: /var/log/sentry-error.log

sentry_nginx_timeout: 15s
sentry_nginx_body_size: 150k

# The following parameters are for toggling dependencies
redis_enabled: yes
nginx_enabled: yes
python_enabled: yes
```

#### Usage

Add `lepture.sentry` to your roles and set vars in your playbook file.

Example:

```yaml

- hosts: all
  sudo: true

  roles:
  - DavidWittman.redis
  - Stouts.nginx
  - Stouts.python
  - Stouts.supervisor
  - ANXS.postgresql
  - lepture.sentry
```

#### License

Licensed under the MIT License. See the LICENSE file for details.

This is a fork from Stouts.sentry.
