OpenStack-Ansible Keystone
##########################

Ansible role that installs and configures OpenStack Keystone. Keystone is
installed behind the Apache webserver listening on port 5000 and port 35357 by
default.

Default Variables
=================

.. literalinclude:: ../../defaults/main.yml
   :language: yaml
   :start-after: under the License.


Required Variables
==================

This list is not exhaustive at present. See role internals for further
details.

.. code-block:: yaml

    # hostname or IP of load balancer providing external network
    # access to Keystone
    external_lb_vip_address: 10.100.100.102

    # hostname or IP of load balancer providing internal network
    # access to Keystone
    internal_lb_vip_address: 10.100.100.102

    # password used by the keystone service to interact with Galera
    keystone_container_mysql_password: "YourPassword"

    keystone_auth_admin_password: "SuperSecretePassword"
    keystone_service_password: "secrete"
    keystone_rabbitmq_password: "secrete"
    keystone_container_mysql_password: "SuperSecrete"

Example Playbook
================

..  code-block:: yaml

    - name: Installation and setup of Keystone
      hosts: keystone_all
      user: root
      pre_tasks:
        - name: Create DB for service
          mysql_db:
            login_user: "root"
            login_password: "secrete"
            login_host: "localhost"
            name: "{{ keystone_galera_database }}"
            state: "present"
          delegate_to: "{{ keystone_galera_address }}"
          when: inventory_hostname == groups['keystone_all'][0]
        - name: Grant access to the DB for the service
          mysql_user:
            login_user: "root"
            login_password: "secrete"
            login_host: "localhost"
            name: "{{ keystone_galera_database }}"
            password: "{{ keystone_container_mysql_password }}"
            host: "{{ item }}"
            state: "present"
            priv: "{{ keystone_galera_database }}.*:ALL"
          with_items:
            - "localhost"
            - "%"
          delegate_to: "{{ keystone_galera_address }}"
          when: inventory_hostname == groups['keystone_all'][0]
      roles:
        - { role: "os_keystone", tags: [ "os-keystone" ] }
      vars:
        external_lb_vip_address: 10.100.100.102
        internal_lb_vip_address: 10.100.100.102
        keystone_galera_address: 10.100.100.101
        keystone_galera_database: keystone
        keystone_venv_tag: "testing"
        keystone_developer_mode: true
        keystone_git_install_branch: master
        keystone_auth_admin_password: "SuperSecretePassword"
        keystone_service_password: "secrete"
        keystone_rabbitmq_password: "secrete"
        keystone_container_mysql_password: "SuperSecrete"
        keystone_rabbitmq_port: 5671
        keystone_rabbitmq_userid: keystone
        keystone_rabbitmq_vhost: /keystone
        keystone_rabbitmq_servers: 10.100.100.101
        keystone_rabbitmq_use_ssl: true
        galera_client_drop_config_file: false

Tags
====

This role supports two tags: ``keystone-install`` and ``keystone-config``

The ``keystone-install`` tag can be used to install and upgrade.

The ``keystone-config`` tag can be used to maintain configuration of the
service.

