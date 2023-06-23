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
- community.postgresql.postgresql_db
- community.postgresql.postgresql_user
- community.postgresql.postgresql_slot
- community.postgresql.postgresql_query
- community.postgresql.postgresql_ext
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
Feature                 | Tag
------------------------|---------------------
Uninstallation          | uninstall
Auto tune (with pgtune) | autotune, auto-tune
Installation            | install
Datadir initialization  | init,initialize,initialise
Configuration           | config, configure, configuration
Replication             | repli, replication
Backup                  | backup
User management         | user, users
Database management     | db, database, databases
Extensions management   | ext, extension, extensions


Linux/PostgreSQL versions supported
-----

Linux/PostgreSQL     |  11  |  12  |  13  |  14  |  15  
------------------|:----:|:----:|:----:|:----:|:----:
Debian 10         | No   | No   | No  | No  | No
Debian 11         | No   | No   | No  | No  | No
Debian 12         | No   | No   | No  | No  | No
Ubuntu 18.04      | No  | No  | No  | No  | No
Ubuntu 20.04      | No  | No  | No  | No  | No
Ubuntu 22.04      | No   | No   | No   | No  | No
CentOS 8 | No  | No  | No  | No  | No
CentOS Stream8  | No  | No  | No  | No  | No
Fedora 35| No   | No   | No  | No  | No
Fedora 36| No   | No   | No  | No  | No
Fedora 37| No   | No   | No  | No  | No
Fedora 38| No   | No   | No  | No  | No
Redhat 8,9        | No  | No  | No  | No  | No

### Proxy usage
----
This role supports use of proxies.

The variables `postgresql_http_general_proxy` and `postgresql_https_general_proxy` can be used to specify a proxy for general internet access (such as downloading files).

The variables `postgresql_http_pkg_proxy` and `postgresql_https_pkg_proxy` can be used to specify a proxy for package manager interaction (such as downloading packages or updating cache).

Note: These variables are translated to environnement variables http_proxy and https_proxy which are passed to corresponding tasks.

