Ansible MariaDB role
=====

This role installs and configures MariaDB.

Requirements
------------

This role requires Ansible 1.6 or higher and platform requirements are listed
in the metadata file.

Role Variables
--------------

The variables that can be passed to this role:

```
# MariaDB repository
mariadb_set_repository: True
mariadb_version: 10.0
mariadb_repo: 'deb http://ftp.igh.cnrs.fr/pub/mariadb/repo/{{mariadb_version}}/debian {{ansible_distribution_release}} main'
mariadb_repo_key_id: '1BB943DB'
mariadb_repo_key_url: 'keyserver.ubuntu.com'

# MariaDB Packages
mariadb_package_client: mariadb-client
mariadb_package_server: mariadb-server

# MariaDB service
mariadb_manage_service: True
mariadb_service_name: mysql

# MariaDB/MySQL tools
mariadb_install_tools: False

# MariaDB users
mariadb_user_home: /root
mariadb_root_username: root
mariadb_root_password: root
mariadb_client_port: 3306

# MariaDB Configuration
mariadb_configuration: /etc/mysql/my.cnf
mariadb_includedir: /etc/mysql/conf.d/

# Configuration vars
mariadb_datadir: /var/lib/mysql

mariadb_default_config:
  - name: 'client'
    config:
      - port = {{mariadb_client_port}}
      - socket = /var/run/mysqld/mysqld.sock
  - name: 'mysqld_safe'
    config:
      - safe_socket = /var/run/mysqld/mysqld.sock
      - safe_nice = 0
  - name: 'mysqld'
    config:
      - user = mysql
      - pid_file = /var/run/mysqld/mysqld.pid
      - socket = /var/run/mysqld/mysqld.sock
      - port = 3306
      - basedir = /usr
      - datadir = "{{mariadb_datadir}}"
      - tmpdir = /tmp
      - skip_external_locking = True
      - bind_address = 127.0.0.1
      - key_buffer = 16M
      - max_allowed_packet = 16M
      - thread_stack = 192K
      - thread_cache_size = 8
      - myisam_recover = BACKUP
      - max_connections = 1000
      - query_cache_limit = 1M
      - query_cache_size = 16M
      - general_log_file = /var/log/mysql/mysql.log
      - general_log = 0
      - slow_query_log = 0
      - slow_query_log_file = /var/log/mysql/mysql-slow.log
      - long_query_time = 1
      - log_queries_not_using_indexes = False
      - default_storage_engine = InnoDB
      - innodb_buffer_pool_size = 128M
      - innodb_log_file_size = 128M
      - innodb_log_buffer_size = 8M
      - innodb_thread_concurrency = 64
      - innodb_read_io_threads = 16
      - innodb_write_io_threads = 16
      - innodb_file_per_table = 1
      - innodb_open_files = 400
      - innodb_io_capacity = 600
      - innodb_lock_wait_timeout = 60
      - innodb_flush_method = O_DIRECT
      - innodb_doublewrite = 0
      - innodb_use_native_aio = 0
      - server_id = 1
      - log_bin = /var/log/mysql/mysql-bin.log
      - expire_logs_days = 10
      - max_binlog_size = 100M
  - name: 'mysqldump'
    config:
      - quick
      - quotes-names
      - max_allowed_packet = 16M
  - name: 'isamchk'
    config:
      - key_buffer = 16M

# Databases
# mariadb_databases:
#   - { name: example, collation: utf8_general_ci, encoding: utf8, replicate: 1 }
mariadb_databases: []

# Users
# mariadb_users:
#   - { name: example, host: 127.0.0.1, password: secret, priv: *.*:USAGE }
mariadb_users: []
```

Examples
========

```
# Roles
- name: mariadb
  hosts: mariadb
  user: root
  roles:
    - deimosfr.mariadb
```

Dependencies
------------

License
-------

GPL2

Author Information
------------------

Pierre Mavro / deimosfr
