Postgres
=========

A role for postgresql db server

Requirements
------------

Debian OS

Role Variables
--------------

- `db_name`
- `db_port`
- `db_user`
- `db_priv` - comma seperated role privilidges; default: `NOSUPERUSER` 
- `postgis_ext` - if true, installs postgis extension
- `postgres_ver` - postgresql server version; default: 9.5
- `postgis_ver` - postgis version; default: 2.3

Dependencies
------------


Example Playbook
----------------

    - hosts: servers
      roles:
         - role: postgres
           db_name: mydb
           db_user: dbuser
           db_port: 5432
           postgis: yes

License
-------

BSD
