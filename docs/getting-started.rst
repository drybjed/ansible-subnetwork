Ansible tags
------------

You can use Ansible ``--tags`` or ``--skip-tags`` parameters to limit what
tasks are performed during Ansible run. This can be used after host is first
configured to speed up playbook execution, when you are sure that most of the
configuration has not been changed.

Available role tags:

``role::subnetwork``
  Main role tag, should be used in the playbook to execute all of the role
  tasks as well as role dependencies.

``type::dependency``
  This tag specifies which tasks are defined in role dependencies. You can use
  this to omit them using ``--skip-tags`` parameter.

``depend-of::subnetwork``
  Execute all ``debops.subnetwork`` role dependencies in its context.

``depend::ifupdown:subnetwork``
  Run ``debops.ifupdown`` dependent role in ``debops.subnetwork`` context.

``depend::ferm:subnetwork``
  Run ``debops.ferm`` dependent role in ``debops.subnetwork`` context.
