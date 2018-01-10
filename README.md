Postgres
=========

A role for postgresql db server

Requirements
------------

Debian or Ubuntu OS

Role Variables
--------------

- `postgres_dbs` - list of mappings with following keys:
    - `name` database name
    - `owner` role name owning given database
    - `exts` list of postgres extensions available for db
    - `privs` list of mappings:
        - `roles` - comma seperated role names (string)
        - `privs` - comma seperated database priviledges: `SELECT`, `UPDATE`, `INSERT` etc.
- `postgres_roles` - list of mappings w/ following keys:
    - `name` - role name
    - `pass` - role password
    - `privs` - comma sepecated role priviledges: `[NO]SUPERUSER`, `[NO]CREATEROLE`, `[NO]CREATEUSER`,
        `[NO]CREATEDB`, `[NO]INHERIT`, `[NO]LOGIN`, `[NO]REPLICATION`,
        `[NO]BYPASSRLS`
- `postgres_ver` - postgresql server version; default: 9.5
- `postgres_exts` - postgres extensions to install: like similarity, unit, pgrouting etc.
  note that for some extensions such as postgis you need to specify also version for ex. `postgis-2.4`
- `postgres_install_os` - yes/no - install OS postgres package rather than from PGDG
  (which is the default source)

see `default.yml` for more

Example Playbook
----------------

    - hosts: dbservers
      roles:
        - role: postgres
          postgres_roles:
            - name: myuser
              pass: secretpass
          postgres_dbs:
            - name: mydb
              owner: myuser
              exts: [unaccent,hstore,postgis]
              privs: []
          postgres_exts: [postgis-2.4]

License
-------

BSD
