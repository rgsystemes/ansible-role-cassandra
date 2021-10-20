cassandra
=========

[![Build Status](https://travis-ci.com/fidanf/ansible-role-cassandra.svg?branch=master)](https://travis-ci.com/fidanf/ansible-role-cassandra)

Install and configures **Apache Cassandra 3.x** along with a JMX Prometheus exporter on **Debian/Ubuntu**.

Tested with :
- Debian 10.x :heavy_check_mark:
- Ubuntu 20.04.x :heavy_check_mark:

Testing
-------

In order to stay as relevant as possible with real-life usages, this role uses [molecule-vagrant](https://github.com/ansible-community/molecule-vagrant) provider instead of the default docker provider from molecule v3.

A list of dependencies necessary to reproduce the testing environment locally can be retrieved from the *.travis.yml* file

Requirements
------------

- Ansible >=2.10

Role Variables
--------------

See [./defaults/main.yml](./defaults/main.yml) for available variables.

Dependencies
------------

None but it is recommended to use the following role to install Java 8 : [gantsign.java](https://github.com/gantsign/ansible-role-java)

Example playbook
----------------

```yaml
---
- hosts: cassandra
  gather_facts: yes
  become: yes

  roles:
    - name: gantsign.java
      vars:
        java_version: 8
        java_is_default_installation: yes
    - name: fidanf.cassandra
      vars:
        cassandra_seeds: 
          - cass01
          - cass02

```

License
-------

MIT / BSD
