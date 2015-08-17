ansible-munin-node
==================

[![Ansible Galaxy](http://img.shields.io/badge/ansible--galaxy-munin--node-blue.svg)](https://galaxy.ansible.com/list#/roles/4799)

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
      -
        name: "apt"
        vars: |
          user root
      -
        name: "mysql*"
        vars: |
          user root
          env.mysqlopts --defaults-file=/etc/mysql/debian.cnf
          env.mysqluser debian-sys-maint
          env.mysqlconnection DBI:mysql:mysql;mysql_read_default_file=/etc/mysql/debian.cnf
      -
        name: "df*"
        vars: |
          env.warning 92
          env.critical 98
          env.exclude_re ^/run/user
      -
        name: "hddtemp_smartctl"
        vars: |
          user root
      -
        name: "if_*"
        vars: |
          user root
      -
        name: "if_err_*"
        vars: |
          user root
      -
        name: "ipmi_*"
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

Example Playbook
----------------

    - hosts: servers
      roles:
         - { role: 0x46616c6b.munin-node }

License
-------

GPLv3
