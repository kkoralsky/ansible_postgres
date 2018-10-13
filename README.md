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
- `postgres_exts` - postgres extensions to install: like similarity, unit, pgrouting etc.
  note that for some extensions such as postgis you need to specify also version for ex. `postgis-2.4`
- `postgres_remotes` - list of IPs along with their netmasks that should be authorized to reach
  this server (separated by space); this can also be in CIDR notation: `<address>/<mask length>`
- `postgres_version` - postgresql server version; default: 9.5
- `postgres_install_os` - yes/no - install OS postgres package rather than from PGDG
  (which is the default source)
- `postgres_replication` - dictionary w/ replication configuration
    - `primary_host` - hostname or IP of the *primary*/*master* postgres server (required)
    - `secondary_host` - hostname or IP of the *secondary*/*standby* server (required)
    - `subnet_length` - subnet length needed for CIDR notation; if defined both
      above keys are meant to be IPs, otherwise hostnames  
    - `streaming` - if defined, streaming replication is performed
    - `hot_standby` - allow readonly queries on standby  
    - `archive_dest` - destination of the archive on the standby server;
      if not defined, `streaming` key should be defined
    - `password` - password for replication user; if not defined - no authentication is required
    - `sleep` - sleep time for `pg_standby` (default: 5)
- `postgres_standby` - provisioned host is *standby*/*secondary* 
- `postgres_basebackup` - if true - we're **purging** postgres data directory and performing `pg_basebackup`
  from master (taking `postgres_replication.primary_host` as a source)

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
