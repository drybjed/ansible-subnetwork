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

        - role: debops.subnetwork/env
          tags: [ 'role::subnetwork' ]

        - role: debops.ifupdown
          tags: [ 'role::ifupdown' ]
          ifupdown_dependent_interfaces: '{{ subnetwork__ifupdown__dependent_list }}'

        - role: debops.ferm
          tags: [ 'role::ferm' ]
          ferm_forward: '{{ subnetwork__kernel_forwarding|d() | bool }}'
          ferm_dependent_rules:
            - '{{ subnetwork__ferm__dependent_rules }}'

        - role: debops.subnetwork
          tags: [ 'role::subnetwork' ]
