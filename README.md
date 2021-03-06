systemd-install-unit
====================

Install and setup systemd units.


This role expects each unit to be describe by an item following the schema below:

```yaml
- directory: "/some/other/path"    # The directory in which install the unit (default: `systemd_unit_install_dir`)
  remote: true                     # Whether the path in `unit` related to the local or the remote machine host (default: False)
  unit: "/path/to/unit.service"    # The path to the unit file (_with_ the `.service`, `.timer, ...).
  user: true                       # Whether to install/run the service as a per-user (not system-wide) service.
  state: "started"                 # Whether the unit should set to start on next boot, immediately or never.
                                   # Admissible values: "disabled" (don't start), "enabled" (start on next boot) or "started" (start immediately)
  tasks:                           # List of strings; Used to instanciate systemd templates (default is `[]`)
    - "first"
    - "second"
```

Note that the units will be install, via Ansible's [template]()


Requirements
------------

This role only applies to system running systemd, obviously. That means
Linux system, only distros who adopted it: Debian (and derivatives),
RedHat (and derivatives), ...


Role Variables
--------------

- `systemd_unit_install_dir`: the directory in which install units;
- `systemd_unit_install_units`: the list of units to install described as explained above;
- `systemd_unit_install_state`: the default state to set units in (default: 'enabled')
- `systemd_unit_install_remote`: whether unit file are to be taken from the local or the remote host (default: False).
- `systemd_unit_install_user`: whether units are to be installed system-wide (False) or as per-user services (True) by default (default: False).



Dependencies
------------

This role has no dependency.


Example Playbook
----------------


```yaml
- hosts: servers
  roles:
    # install a bunch of units from files on the remote machine
    - role: cans.systemd-install-unit
      systemd_unit_install_remote: True

    # install a bunch of units from local files
    - role: cans.systemd-install-unit
      systemd_unit_install_units:
        - unit: units/wonderful.service
          directory: /usr/local/etc/systemd/system
        - unit: units/critical.service
          state: "started"
```


License
-------

GPLv2


Author Information
------------------

Copyright © 2017-2018, Nicolas CANIART.
