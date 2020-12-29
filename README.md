ansible-munin-node
==================

[![Build Status](https://github.com/systemli/ansible-role-munin-node/workflows/Integration/badge.svg?branch=master)](https://github.com/systemli/ansible-role-munin-node/actions?query=workflow%3AIntegration)
[![Ansible Galaxy](http://img.shields.io/badge/ansible--galaxy-munin__node-blue.svg)](https://galaxy.ansible.com/systemli/munin_node/)

Role to install & maintain Munin Node

Role Variables
--------------

The playbook requires no special configuration, but offers a bunch of options.

Defaults:

    munin_node_packages:
      - munin-node
      - munin-plugins-extra
      - git

    munin_node_extra_plugins: True

    munin_node_plugins_active:
      - cpu
      - df
      - load
      - memory
      - uptime
      - users
      - swap
      - threads

    munin_node_plugins_disabled:
      - diskstats
      - df_inode
      - iostat_ios

    munin_node_extra_plugins_active: []

    munin_plugin_config:
      - name: amavis
        vars: |
          group adm
          env.MUNIN_MKTEMP /bin/mktemp -p /tmp/ $1
          env.amavislog /var/log/mail.info
      - name: "apt"
        vars: |
          user root
      - name: courier_mta_mailqueue
        vars: |
          group daemon
      - name: courier_mta_mailstats
        vars: |
          group adm
      - name: courier_mta_mailvolume
        vars: |
          group adm
      - name: "cps*"
        vars: |
          user root
      - name: "df*"
        vars: |
          env.warning 92
          env.critical 98
          env.exclude_re ^/run/user
      - name: exim_mailqueue
        vars: |
          group adm, (Debian-exim)
      - name: exim_mailstats
        vars: |
          group adm, (Debian-exim)
          env.logdir /var/log/exim4/
          env.logname mainlog
      - name: "fw_conntrack"
        vars: |
          user root
      - name: "fw_forwarded_local"
        vars: |
          user root
      - name: "hddtemp_smartctl"
        vars: |
          user root
      - name: "hddtemp2"
        vars: |
          user root
      - name: "if_*"
        vars: |
          user root
      - name: "if_err_*"
        vars: |
          user nobody
      - name: "ip_*"
        vars: |
          user root
      - name: "ipmi_*"
        vars: |
          user root
      - name: "mysql*"
        vars: |
          user root
          env.mysqlopts --defaults-file=/etc/mysql/debian.cnf
          env.mysqluser {{ lookup('vars', 'munin_node_mysql_mysqluser_'+ansible_distribution_release,default='debian-sys-maint') }}
          env.mysqlconnection DBI:mysql:mysql;mysql_read_default_file=/etc/mysql/debian.cnf
      - name: postfix_mailqueue
        vars: |
          user postfix
      - name: postfix_mailstats
        vars: |
          group adm
      - name: postfix_mailvolume
        vars: |
          group adm
          env.logfile mail.log
      - name: sendmail_*
        vars: |
          user smmta
      - name: "smart_*"
        vars: |
          user root
      - name: "vlan*"
        vars: |
          user root
      - name: "ejabberd*"
        vars: |
          user ejabberd
          env.statuses available away chat xa
          env.days 1 7 30
      - name: "jmx_*"
        vars: |
          env.ip 127.0.0.1
          env.port 5400
      - name: samba
        vars: |
          user root
      - name: "munin_stats"
        vars: |
          user munin
          group munin
      - name: "postgres_*"
        vars: |
          user postgres
          env.PGUSER postgres
          env.PGPORT 5432
      - name: fail2ban
        vars: |
          user root

    munin_node_config_log_level: 4
    munin_node_config_log_file: /var/log/munin/munin-node.log
    munin_node_config_pid: /var/run/munin/munin-node.pid
    munin_node_config_background: 1
    munin_node_config_setid: 1
    munin_node_config_user: root
    munin_node_config_group: root
    munin_node_config_global_timeout: 900
    munin_node_config_timeout: 60
    munin_node_config_host_name: "{{ ansible_fqdn }}"
    munin_node_config_allow: |
      allow ^127\.0\.0\.1$
      allow ^::1$
    munin_node_config_host: "*"
    munin_node_config_port: 4949
    munin_node_plugins_git_update: "no

Download
--------

Download latest release with `ansible-galaxy`

	ansible-galaxy install systemli.munin_node

Example Playbook
----------------

    - hosts: servers
      roles:
         - { role: systemli.munin_node }


Testing & Development
---------------------

Tests
-----

For developing and testing the role we use Github Actions, Molecule, and Vagrant. On the local environment you can easily test the role with

Run local tests with:

```
molecule test
```

Requires Molecule, Vagrant and `python-vagrant, molecule-goss, molecule-vagrant` to be installed.For developing and testing the role we use Travis CI, Molecule and Vagrant. On the local environment you can easily test the role with



License
-------

GPLv3

Author Information
------------------

https://www.systemli.org
