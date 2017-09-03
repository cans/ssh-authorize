cans.ssh-authorize
==================

A simple role to setup SSH authorization on user account in bulk

Given a directory (`public_keys` by default) that contains a set of
files each containing a public key, merges those files to produce the
`~/.ssh/authorized_keys` file on the remote user account.

The actual file created depends on the value of several variables:

    "{{sshauthz_homes_dir}}/{{sshauthz_user}}/{{sshauthz_ssh_config_dirname}}/{{sshauthz_authorized_keys_filename}}"

Which allows for fairly particualar setup if needed. But of course
default value for those variable should be applicable for most setups.

Note: As this role operates in a quite blunt fashion, for efficiency
reasons, to avoid locking you out of the machine, this role offers
a mecanism to check you key exists in the source directory


Requirements
------------

This role has no particular requirements.


Role variables
--------------

### Defaults

- `sshauthz_homes_dir: base directory for user accounts on the remote
  host (default: "/home")
- `sshauthz_user`: the remote account for which set authorizations
  (default: "{{ansible_user_id}}")
- `sshauthz_ssh_config_dirname`: (default: ".ssh")
- `sshauthz_authorized_keys_filename`: Name of the authorization file
  on the remote host (default: "authorized_keys")
- `sshauthz_keys_directory`: directory in which find the keys to
  authorize on the remote account (default "{{ playbook_dir + '/files/public_keys/default' }}")


Dependencies
------------

This roles has no dependency.


Example playbook
----------------

```yaml
- hosts: etl, proxy
  roles:
    - { role: ssh-access, sshauthz_user: remus }
    - { role: ssh-access, sshauthz_user: romolus, sshauthz_keys_directory: '~/public_keys' }
```


License
-------

GPLv2


Author Information
------------------

Copyright © 2017, Nicolas CANIART.
