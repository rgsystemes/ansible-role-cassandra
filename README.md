cassandra
=========

[![Build Status](https://travis-ci.com/fidanf/ansible-role-cassandra.svg?branch=master)](https://travis-ci.com/fidanf/ansible-role-cassandra)

Install and configures Apache Cassandra 3.x along with a JMX Prometheus exporter on **Debian/Ubuntu**.

Tested with :
- Debian 10.x :heavy_check_mark:
- Ubuntu 20.04.x :heavy_check_mark:
- Ubuntu 18.04.x :heavy_check_mark:

Testing
-------

In order to stay as relevant as possible with real-life usages, this role uses [molecule-vagrant](https://github.com/ansible-community/molecule-vagrant) provider instead of the default docker provider from molecule v3.

A list of dependencies necessary to reproduce the testing environment locally can be retrieved from the *.travis.yml* file

Requirements
------------

- Ansible >=2.9

Role Variables
--------------

```yaml
---
# apt
cassandra_base_path: /etc/cassandra
cassandra_configuration_file_path: /etc/cassandra/cassandra.yaml
cassandra_repo_apache_release: 311x
cassandra_configure_apache_repo: true
cassandra_task_delay: 10
cassandra_task_retries: 5
cassandra_disable_swap: yes
cassandra_15770_workaround: yes # fixes init.d with impacted distributions. https://issues.apache.org/jira/browse/CASSANDRA-15770

# cassandra-rackdc.properties
cassandra_dc: DC1
cassandra_rack: RACK1

# cassandra.yaml
cassandra_cluster_name: AcmeCluster
cassandra_num_tokens: 16
cassandra_endpoint_snitch: GossipingPropertyFileSnitch
cassandra_enable_materialized_views: false
cassandra_enable_sasi_indexes: false
cassandra_seeds: [] # this must be overriden with hostnames/IPs of your choice
# cassandra_node_ip: # this can be explicitly set using host_vars, otherwise it takes it's value from to ansible_host if defined in IPv4, else from inventory_hostname

# exporter
cassandra_exporter_release_version: 0.12.0
cassandra_exporter_release_url: "https://repo1.maven.org/maven2/io/prometheus/jmx/jmx_prometheus_javaagent/{{ cassandra_exporter_release_version }}/jmx_prometheus_javaagent-{{ cassandra_exporter_release_version }}.jar"
cassandra_exporter_jvm_opts: "JVM_OPTS=\"$JVM_OPTS -javaagent:{{ cassandra_base_path }}/jmx_prometheus_javaagent-{{ cassandra_exporter_release_version }}.jar=7070:{{ cassandra_base_path }}/prometheus_java_exporter.yml\""

# restart
cassandra_restart_enabled: yes

```

Dependencies
------------

None but it is recommended to use the following role to install java 8 : [gantsign.java](https://github.com/gantsign/ansible-role-java)

Example playbook
----------------

```yaml
---
- hosts: cassandra
  gather_facts: yes
  become: yes

  tasks:
    - import_role: 
        name: gantsign.java
      vars:
        java_version: '8' # or jdk8u252
        java_is_default_installation: yes
        java_use_local_archive: no
        java_install_dir: '/opt/java'
      tags: ['dependencies', 'java']
    - name: Ensure correct java version is selected
      alternatives:
        link: /usr/bin/java
        name: java
        path: "{{ ansible_local.java.general.home }}/bin/java"
        priority: 1
    - import_role: 
        name: rgsystem.cassandra

```

License
-------

MIT / BSD
