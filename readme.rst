os_keystone role
#############
:tags: openstack, cloud, ansible, os_keystone
:category: \*nix

os_keystone Role

.. code-block:: yaml

    - name: os_keystone role
      hosts: "hosts"
      user: root
      roles:
        - { role: "os_keystone" }


Note. The template role has the template name within it. Please change the name 
throughout the code base.

.. code-block:: bsah

    find . -type f -exec sed -i 's/os_keystone/CHANGE_ME_PLEASE/g' {} \;
