Getting started
===============

.. contents::
   :local:


Example inventory
-----------------

To run ``debops.subnetwork`` on host given in
``debops_service_subnetwork`` Ansible inventory group:

.. code:: ini

    [debops_service_subnetwork]
    hostname

Example playbook
----------------

Here's an example playbook that can be used to configure local networks::

    ---

    - name: Configure internal networks
      hosts: [ 'debops_service_subnetwork', 'debops_subnetwork' ]
      become: True

      roles:

        - role: debops.ifupdown
          tags: [ 'role::ifupdown' ]
          ifupdown_dependent_interfaces: '{{ subnetwork__ifupdown__dependent_list }}'
          when: subnetwork__addresses

        - role: debops.ferm
          tags: [ 'role::ferm' ]
          ferm_forward: '{{ subnetwork__kernel_forwarding|d() | bool }}'
          ferm_dependent_rules:
            - '{{ subnetwork__ferm__dependent_rules if subnetwork__addresses else [] }}'

        - role: debops.subnetwork
          tags: [ 'role::subnetwork' ]

