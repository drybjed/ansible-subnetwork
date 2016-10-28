Changelog
=========

.. include:: includes/all.rst

**debops.subnetwork**

This project adheres to `Semantic Versioning <http://semver.org/spec/v2.0.0.html>`__
and `human-readable changelog <http://keepachangelog.com/en/0.3.0/>`__.

The current repository maintainer is drybjed_.


`debops.subnetwork master`_ - unreleased
----------------------------------------

.. _debops.subnetwork master: https://github.com/debops/ansible-subnetwork/compare/v0.2.0...master

Fixed
~~~~~

- Support to configure IPv6 addresses on the first role run. Previously, the
  role needed a second Ansible run for that. [ypid_]


`debops.subnetwork v0.2.0`_ - 2016-03-23
----------------------------------------

.. _debops.subnetwork v0.2.0: https://github.com/debops/ansible-subnetwork/compare/v0.1.1...v0.2.0

Added
~~~~~

- Make the Firewall configuration more flexible for the subnetwork by
  introducing :envvar:`subnetwork__allow_incoming_connections`,
  :envvar:`subnetwork__allow_incoming_interfaces` and
  :envvar:`subnetwork__allow_outgoing_interfaces`. [ypid_]

Changed
~~~~~~~

- Renamed ``subnetwork_ifupdown_interfaces`` to
  :envvar:`subnetwork__ifupdown__dependent_list`. [ypid_]

- Use :envvar:`subnetwork__ifupdown__dependent_list` to generate Firewall entries
  instead of templating a file under :file:`/etc/ferm`. [ypid_]

- Remove most of the Ansible role dependencies, leaving only those that are
  required for the role to run correctly.

  Configuration of dependent services like firewall, TCP Wrappers, APT
  preferences is set in separate default variables. These variables can be used
  by Ansible playbooks to configure settings related to ``subnetwork`` in other
  services. [ypid_]

- Changed namespace from ``subnetwork_`` to ``subnetwork__``.
  ``subnetwork_[^_]`` variables are hereby deprecated and you might need to
  update your inventory. This oneliner might come in handy to do this.

  .. code:: shell

     git ls-files -z | find -type f -print0 | xargs --null sed --in-place --regexp-extended 's/(subnetwork)_([^_])/\1__\2/g'

  [ypid_]

- Documented basic usage of the role. [ypid_]

- Enable packet forwarding through debops.ferm_ only when subnetwork
  addresses are defined. [drybjed_]

- Change the default ``ifupdown`` configuration "weight" to put the subnetwork
  bridge after other bridges. [drybjed_]

- Render the debops.ferm_ configuration only when :envvar:`subnetwork__addresses`
  has value, this fixes an issue when Ansible stops with an error when there
  are no addresses set. [drybjed_]

- Update documentation. [drybjed_]

Removed
~~~~~~~

- Remove the assert check in ``debops.subnetwork/env`` and replace it with
  conditional role execution in the playbook. [drybjed_]


`debops.subnetwork v0.1.1`_ - 2015-06-02
----------------------------------------

.. _debops.subnetwork v0.1.1: https://github.com/debops/ansible-subnetwork/compare/v0.1.0...v0.1.1

Added
~~~~~

- Added ``subnetwork_bridge_iface_regex`` variable to allow to use different
  ``subnetwork_iface`` names without modifying the default
  ``subnetwork_ifupdown_interfaces``. [ypid_]

Changed
~~~~~~~

- Role is updated to use new features in debops.ifupdown_:

  Most of the "logic" behind how IP addresses were configured is now included
  in debops.ifupdown_. Instead of having separate variables for IPv4 and
  IPv6 addresses, you now should use ``subnetwork_addresses`` list to specify
  IPv4/IPv6 subnets to configure, in the "host/prefix" format.

  Names of generated files in :file:`/etc/network/interfaces.d/` have been changed
  to no longer contain duplicate ``_ipv4_ipv4`` or ``_ipv6_ipv6`` suffixes,
  since debops.ifupdown_ automatically adds the network suffix. You need to
  remove old configuration files from :file:`/etc/network/interfaces.config.d/` to
  prevent any problems with duplicate interface configuration.

  ``subnetwork_ipv{4,6}_options`` variables have been renamed to
  ``subnetwork_options{6}``. By default, bridge will be configured with
  "forward delay" 0 to help with DHCP requests.

  ``subnetwork_ifupdown_prepend_interfaces`` list has been removed.

  ``subnetwork_ipv4_gateway`` variable has been renamed to
  ``subnetwork_ipv4_nat_snat_interface`` to better reflect its purpose as it
  specifies the interface from which firewall should take an IPv4 address to
  use in SNAT directive. [drybjed_]


debops.subnetwork v0.1.0 - 2015-05-12
-------------------------------------

Added
~~~~~

- Add Changelog. [drybjed_]

- Disable automatically added ``allow-hotplug`` option in IPv6 bridge section.
  IPv6 is enabled automatically with IPv4 when bridge is brought up. [drybjed_]

- Redefine ``subnetwork_ipv{4,6}_options`` variables.

  When you use multiple subnetwork interfaces, additional options provided in
  above variables for 1 interface would propagate into the rest of the
  interfaces. To change that, options variables are redefined as dicts with
  keys specifying the interface name and values being YAML text blocks with
  options.

  Note that dict keys must be specified explicitly, not as Jinja variables. See
  :file:`defaults/main.yml` file for examples. [drybjed_]
