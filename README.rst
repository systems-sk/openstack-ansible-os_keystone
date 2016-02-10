OpenStack keystone
##################
:tags: openstack, keystone, cloud, ansible
:category: \*nix

Role to install keystone. This will install keystone using apache.

This role will install the following:
    * keystone
    * apache2

..  code-block:: yaml

    - name: Installation and setup of Keystone
      hosts: keystone_all
      user: root
      roles:
        - { role: "os_keystone", tags: [ "os-keystone" ] }
      vars:
        keystone_galera_address: "{{ internal_lb_vip_address }}"
