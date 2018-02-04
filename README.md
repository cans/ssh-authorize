cans.ssh-authorize
==================

[![Build Status](https://travis-ci.org/cans/ssh-authorize.svg?branch=master)](https://travis-ci.org/cans/ssh-authorize)
[![Ansible Galaxy](https://img.shields.io/badge/ansible--galaxy-cans.package--source-blue.svg?style=flat-square)](https://galaxy.ansible.com/cans/ssh-authorize)
[![License](https://img.shields.io/badge/license-GPLv2-brightgreen.svg?style=flat-square)](LICENSE)

A simple role to setup SSH authorization on user account in bulk

Given a directory (`public_keys` by default) that contains a set of
files each containing a public key, merges those files to produce the
`~/.ssh/authorized_keys` file on the remote user account.

The actual file created depends on the value of several variables:

    "{{sshauthz_homes_dir}}/{{sshauthz_user}}/{{sshauthz_ssh_config_dirname}}/{{sshauthz_authorized_keys_filename}}"

Which should allow for fairly peculiar setup if needed. But of course
default value for those variables should be applicable for most setups
(see below).

Note: As this role operates in a quite blunt fashion, for efficiency
reasons, to avoid locking yourself out of the machine, it offers
a mecanism to check that your own key exists in the source directory
(_cf._ the `sshauthz_user_key_name` variable below).


Requirements
------------

This role has no particular requirements.


Role variables
--------------

### Input variables

These are the variables you need to set for the role to perform its
task.

- `sshauthz_keys_directory`: directory in which find the keys to
  authorize on the remote account (defaults to `{{ playbook_dir + '/files/public_keys/default' }}`)
- `sshauthz_user`: the remote account for which set authorizations
  (defaults to `{{ansible_user_id}}`)
- `sshauthz_user_key_name`: this is the name of your file that contains
  your own public key. If no file with that name is found in the
  `sshauthz_keys_directory`, then the role will abort, preventing you
  from locking yourself out of the machine. It is a fairly weak test,
  but it proved useful.


### Defaults

These are variables you may want to set in situations where your setup
does not follow standard practices (_e.g._ uses unexpected locations
for certains files, ...)

- `sshauthz_homes_dir`: base directory for user accounts on the remote
  host (default: `/home`)
- `sshauthz_ssh_config_dirname`: (default: `.ssh`)
- `sshauthz_authorized_keys_filename`: Name of the authorization file
  on the remote host (default: `authorized_keys`)
- `sshauthz_user`: the user for which update its `authorized_keys` file
  (defaults to `{{ ansible_user_id }}`)
- `sshauthz_group`: the group to which the `authorized_keys` file, and
  its parent directory should belong to (defaults to
  `{{ ansible_user_gid | default(ansible_user_id) }}`)


Dependencies
------------

This roles has no dependency.


Example playbook
----------------

```yaml
- hosts: etl, proxy
  remote_user: remus
  roles:
    - role: ssh-authorize
```

With the playbook above, you install all keys found under the  directory
pointed by `sshauthz_keys_directory` on *remus'* account, granting those
users access to his account.

```yaml
- hosts: etl, proxy
  remote_user: remus
  roles:
    - role: ssh-authorize
      sshauthz_user: romolus
      sshauthz_keys_directory: '~/public_keys'
      become: true
```

With this playbook, you connect as *remus* but will install keys on
*romulus'* account. Note the `become` option which may or may not be
necessary depending on your setup (you may also use `become_user`)


License
-------

GPLv2


Author Information
------------------

Copyright © 2017, Nicolas CANIART.
