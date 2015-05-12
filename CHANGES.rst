Changelog
=========

v0.1.0
------

*Released: 2015-05-12*

- Add Changelog. [drybjed]

- Disable automatically added ``allow-hotplug`` option in IPv6 bridge section.
  IPv6 is enabled automatically with IPv4 when bridge is brought up. [drybjed]

- Redefine ``subnetwork_ipv{4,6}_options`` variables.

  When you use multiple subnetwork interfaces, additional options provided in
  above variables for 1 interface would propagate into the rest of the
  interfaces. To change that, options variables are redefined as dicts with
  keys specifying the interface name and values being YAML text blocks with
  options.

  Note that dict keys must be specified explicity, not as Jinja variables. See
  ``defaults/main.yml`` file for examples. [drybjed]

