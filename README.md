# Ansible Role - PostgreSQL
[![Maintainer](https://img.shields.io/badge/maintained%20by-claranet-e00000?style=flat-square)](https://www.claranet.fr/)
[![License](https://img.shields.io/github/license/claranet/ansible-role-postgresql?style=flat-square)](LICENSE)
[![Release](https://img.shields.io/github/v/release/claranet/ansible-role-postgresql?style=flat-square)](https://github.com/claranet/ansible-role-postgresql/releases)
[![Status](https://img.shields.io/github/actions/workflow/status/claranet/ansible-role-postgresql/molecule.yml?branch=main&style=flat-square&label=tests)](https://github.com/claranet/ansible-role-postgresql/actions?query=workflow%3A%22Ansible+Molecule%22)
[![Ansible version](https://img.shields.io/badge/ansible-%3E%3D2.10-black.svg?style=flat-square&logo=ansible)](https://github.com/ansible/ansible)
[![Ansible Galaxy](https://img.shields.io/badge/ansible-galaxy-black.svg?style=flat-square&logo=ansible)](https://galaxy.ansible.com/claranet/postgresql)


> :star: Star us on GitHub — it motivates us a lot!

Install and configure PostgreSQL server on Debian,RedHat systems.

## :warning: Requirements

Ansible >= 2.10

## :arrows_counterclockwise: Dependencies
```yaml
- community.postgresql.postgresql_user
- community.postgresql.postgresql_membership
- community.postgresql.postgresql_tablespace
- community.postgresql.postgresql_db
- community.postgresql.postgresql_schema
- community.postgresql.postgresql_table
- community.postgresql.postgresql_owner
- community.postgresql.postgresql_privs
- community.postgresql.postgresql_ext
- community.postgresql.postgresql_script
- community.postgresql.postgresql_query
- community.postgresql.postgresql_slot
```


## :zap: Installation

```bash
ansible-galaxy install claranet.postgresql
```


### PostgreSQL version to be installed 
----
_default is 14_
```yaml
postgresql_version: 14
```

### Available features and tags
-----
This role support the following features and tags in the following order during execution:
Feature                             | Tag
------------------------------------|---------------------
Uninstallation                      | uninstallation
Installation                        | install, installation
Datadir initialization              | init,initialize,initialise
Auto tune (with pg-config.org)      | autotune, auto-tune
Configuration                       | config, configure, configuration
Replication                         | repli, replication
Backup                              | backup
User & membership management        | user, users
Tablespace management               | tblspc, tablespace, tablespaces
Database management                 | db, database, databases
Ownership & privileges management   | owner, owners, ownership, priv, privs, privileges
Extensions management               | ext, extension, extensions
SQL code executions                 | query, script


Linux/PostgreSQL versions supported
-----

Linux/PostgreSQL  |  11  |  12  |  13  |  14  |  15  | 16
------------------|:----:|:----:|:----:|:----:|:----:|:----:
Debian 10         | No   | No   | No   | No   | No   |  No 
Debian 11         | No   | No   | No   | No   | No   |  No 
Debian 12         | No   | No   | No   | No   | No   |  No 
Ubuntu 18.04      | No   | No   | No   | No   | No   |  No 
Ubuntu 20.04      | No   | No   | No   | No   | No   |  No 
Ubuntu 22.04      | No   | No   | No   | No   | No   |  No 
CentOS 8          | No   | No   | No   | No   | No   |  No 
CentOS Stream8    | No   | No   | No   | No   | No   |  No 
Fedora 35         | No   | No   | No   | No   | No   |  No 
Fedora 36         | No   | No   | No   | No   | No   |  No 
Fedora 37         | No   | No   | No   | No   | No   |  No 
Fedora 38         | No   | No   | No   | No   | No   |  No 
Redhat 8          | No   | No   | No   | No   | No   |  No 
Redhat 9          | No   | No   | No   | No   | No   |  No 

### Proxy usage
----
This role supports use of proxies.

The variables `postgresql_http_general_proxy` and `postgresql_https_general_proxy` can be used to specify a proxy for general internet access (such as downloading files).

The variables `postgresql_http_pkg_proxy` and `postgresql_https_pkg_proxy` can be used to specify a proxy for package manager interaction (such as downloading packages or updating cache).

Note: These variables are translated to environnement variables http_proxy and https_proxy which are passed to corresponding tasks.



### Installation
----

### Initialization
----

### Auto tuning
----
This role supports the use of the website [pgconfig.org](https://www.pgconfig.org) for automatically tunning some of configuration settings of the postgresql server.

You can check the [full documentation](https://docs.pgconfig.org/api/#available-parameters) on the available configurations parameters.

Configuration example for variables (_those are the default values_):
```yaml
postgresql_autotune: true
postgresql_autotune_base_url: https://api.pgconfig.org
postgresql_autotune_pg_version: "{{ postgresql_version }}"
# linux/windows/unix
postgresql_autotune_os_type: linux
# 386/x86-64
postgresql_autotune_arch: x86-64
# HDD/SSD/SAN
postgresql_autotune_drive_type: SSD
# WEB/OLTP/DW/Mixed/Desktop
postgresql_autotune_env_name: OLTP
postgresql_autotune_cpus: "{{ ansible_processor_nproc | d('') }}"
# Total ram in GB
postgresql_autotune_total_ram: "{{ ((ansible_memtotal_mb / 1024) | round | int) | d('') }}"
```

### Configure
----

### Physical Replication
----
Configuration example for the primary server: 

```yaml
postgresql_replication: true
postgresql_replication_user: replication_user
postgresql_replication_password: replication_password

postgresql_replication_role: primary
# Used to generate hba rules to allow the specified servers to connect to the primary server
postgresql_replication_replica_addresses: [192.168.1.6/32, 192.168.1.7/32]
```


Cnofiguration example for the replicas:

```yaml
postgresql_replication: true
postgresql_replication_user: replication_user
postgresql_replication_password: replication_password

postgresql_replication_role: replica
postgresql_replication_primary_address: 192.168.1.5
postgresql_replication_primary_inventory_name: node1 # primary server name in the ansible inventory

```

Using slots for replication:

```yaml
postgresql_replication_slot: replica1_slot
postgresql_replication_create_slot: true
```
When set to true the variable `postgresql_replication_create_slot` appends the `--create-slot` to the pg_basebackup command run to copy data from the primary.

Notes:
-----
When using the slot feature for replication, make sure to indicate a different slot for each replica. You can set that value in the host_vars for each server.


### Backup
----
Configuration example for backup.

```yaml
# Allow ansible to setup postgresql backups when running
postgresql_backup: true
# Root directory containing the backups
postgresql_backup_root_dir: /var/backups/postgresql
postgresql_backup_mail_addr: admin@email.com
postgresql_backup_schedule:
    hour: 0
    minute: 0
# 3 days retentions for daily backups
postgresql_backup_brdaily: 3
# disable weekly and monthly backups
postgresql_backup_doweekly: 0
postgresql_backup_domonthly: 0
# Weekly and monthly backups are disabled so these values don't really matter
postgresql_backup_brweekly: 0
postgresql_backup_brmontly: 0
```


### Create/Remove database users
----
Configuration example for managing users:

```yaml
postgresql_users:
# Create two groups 'group1' and 'group2' by making use of thr role_attr_flags attribute
  - name: group1
    role_attr_flags: NOLOGIN
  - name: group2
    role_attr_flags: NOLOGIN
# Create 'user1' and 'user2' with default parameters
  - name: user1
  - name: user2
# Create user 'jdoe' with more personalized parameters
  - name: jdoe
    password: password
    comment: this is a test user
    expires: "Jun 21 2029"

postgresql_memberships:
# Ensure the role 'user1' belongs to group 'group1'
  - groups:
    - group1
    target_roles:
    - user1
    state: present
# Ensure the role 'user2' does not belong to the group 'group2'
  - groups:
    - group2
    target_roles:
    - user2
    state: absent
# Ensure the role 'jdoe' does not belong to any group
  - groups: []
    target_roles:
    - jdoe
    state: exact
```

_Notes:_
Check the links for a documentation on all the available options for defining items within the variables:
- [`postgresql_users`](https://docs.ansible.com/ansible/latest/collections/community/postgresql/postgresql_user_module.html#parameters) - [`postgresql_memberships`](https://docs.ansible.com/ansible/latest/collections/community/postgresql/postgresql_membership_module.html#parameters).


### Tablespaces
----

COnfiguration example for managing tablespaces:
```yaml
postgresql_tablespaces:
# Create tablespace 'ssd'
  - name: ssd
    set:
      random_page_cost: 1
      seq_page_cost: 1
    owner: jdoe
    location: /tmp/ssd
    location_create: true # default is false
    state: present # default is present
    location_owner: postgres # default is postgres
    location_group: postgres # default is postgres
    location_mode: '0700' # default is '0700'
# Delete tablespaces 'temp2'
  - name: temp2
    state: absent
    location: /tmp/temp2_tblspc
    set:
      random_page_cost: 1
    owner: user1
```

_Notes:_
When combining `location_create: true` with `state: present` the role will create the location of the tablespace with the specified permissions before creating the tablespace itself.

If you ensure the existence of that location by others means, feel free to not set the variables `location_*`.


### Create/Remove databases
----

Configuration example for managing databases:

```yaml
postgresql_databases:
  - name: db1
    owner: user1
    encoding: UTF-8
    lc_collate: en_US.UTF-8
    lc_ctype: en_US.UTF-8
    conn_limit: 100
    template: template0
  - name: db2
    owner: user2
  - name: db3
    state: absent

postgresql_schemas:
  - name: acme
    db: db1
  - name: acme
    db: db2
  - name: not_existing_shema
    db: db1
    owner: user1
    state: absent
    cascade_drop: true

postgresql_tables:
  - name: table1
    db: db1
    owner: user1
    columns:
      - id SERIAL PRIMARY KEY
      - name VARCHAR(50)
      - age INT
      - email VARCHAR(100)
    tablespace: ssd
    storage_params:
      - fillfactor=10
      - autovacuum_analyze_threshold=1
  - name: acme.table2
    db: db1
    columns: waste_id int
    unlogged: true
```

_Notes:_
Check the links for a documentation on all the available options for defining items within the variables:
- [`postgresql_database`](https://docs.ansible.com/ansible/latest/collections/community/postgresql/postgresql_database_module.html#parameters)
- [`postgresql_schema`](https://docs.ansible.com/ansible/latest/collections/community/postgresql/postgresql_schema_module.html#parameters)
- [`postgresql_table`](https://docs.ansible.com/ansible/latest/collections/community/postgresql/postgresql_table_module.html#parameters)

### Manage ownership and privileges
----
```yaml
postgresql_privs:
  - roles: group1 # group1 and user1 are granted all privs on all object wihtin the public schema of the example db
    db: db1
    privs: ALL
    objs: table1
    type: table
    # schema: public
    grant_option: true
  - roles: user2 # grant nreslou user all privs on nreslou database by first connecting to the postgres maintenance db
    db: postgres
    type: database
    privs: ALL
    objs: db1,db2
    grant_option: true


postgresql_ownerships:
  - db: db1
    new_owner: user1
    obj_name: table1
    obj_type: table
  - db: db2 # reassign all dbs owned by user1 to user2 and all objects in db2 to user2
    new_owner: user2
    reassign_owned_by: user1
```
_Notes:_
Check the links for a documentation on all the available options for defining items within the variables:
- [`postgresql_ownerships`](https://docs.ansible.com/ansible/latest/collections/community/postgresql/postgresql_owner_module.html#parameters) 
- [`postgresql_privs`](https://docs.ansible.com/ansible/latest/collections/community/postgresql/postgresql_privs_module.html#parameters)



### Extensions management
----
Configuration example for extensions management:

```yaml
postgresql_extensions:
  - name: pg_stat_statements
    db: db1
    cascade: true
    version: latest
    schema: public
  - name: non_existing_extension
    db: db1
    state: absent
```

_Notes:_
For the extensions with `state: present, version: latest`, the role will always report `changed: false` as the underlying module does not differentiate when the extension is actually updated or not.



### SQL executions
----
Configuration example for running sql:
```yaml
postgresql_queries:
  - query: SELECT version()
    db: db1
  - query:
      - select * from public.table1
    db: db1
postgresql_scripts:
  - path: /tmp/insert_in_table1.sql
    db: db1
```

_Notes:_
Check the links for a documentation on all the available options for defining items within the variables:
- [`postgresql_queries`](https://docs.ansible.com/ansible/latest/collections/community/postgresql/postgresql_query_module.html#parameters) - [`postgresql_scripts`](https://docs.ansible.com/ansible/latest/collections/community/postgresql/postgresql_script_module.html#parameters).


### Advanced customized installation
----






## :pencil2: Full Example Playbook

```yaml
---
```


## :gear: Role variables

Variable name                              | Default value                           | Notes                                                                                                        |
------------------------------------------ |-----------------------------------------|--------------------------------------------------------------------------------------------------------------|
postgresql_version                            | "14"                                  |
postgresql_debug                              | false                                   | Controls wether or not to show debug infos. Activating this will potentially make ansible output some postgresql credentials       



PostgreSQL configuration variables
----
The following variables are defined and correspond to their respective postgresql configuration variable used in `postgresql.conf` file without the prefix `postgresql_` .

Variable name                                    | Default value                     | Notes                                                                                                        |
------------------------------------------------ |---------------------------------- |------------------------------------------------------------------------------------------------------------- |
postgresql_port                                     | "5432"                            |


## :closed_lock_with_key: [Hardening](HARDENING.md)

## :heart_eyes_cat: [Contributing](CONTRIBUTING.md)
Checkout the [Contributing](CONTRIBUTING.md) if you are looking for a guide on how to setup an environnement so you can test this role as a developper.


## :copyright: [License](LICENSE)

[Mozilla Public License Version 2.0](https://www.mozilla.org/en-US/MPL/2.0/)

## Author information

Proudly made by the Claranet team and inspired by:
- [Jeff Geerling](https://github.com/geerlingguy/ansible-role-postgresql)
