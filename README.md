Ansible User/Group Management Role
==================================

This ansible role allows for the management of users and groups on a host. It currently is capable of:

* Adding/Updating/Removing users and/or groups
* Providing users sudo access via 'sudoers.d'
* SSH key management for users

Supported Platforms
-------------------

The role was tested with Ansible v1.9.4, and an Ubuntu 14.04 host only. However, nothing in the role should be platform specific, so anything supporting the 'userdel', 'usermod', and 'useradd' commands most likely will work also.

Supported Variables
-------------------

### User Variables

`user_state` - Whether or not you want the user to exist (DEFAULT: 'present'; 'absent' removes user)

`user_username` - The username of the user (**REQUIRED**)

`user_password` - The crypted password of the user (OPTIONAL)

`user_shell` - The shell for the user (DEFAULT: /bin/bash)

`user_uid` - The UID for the user (OPTIONAL)

`user_comment` - A comment to add with the user (OPTIONAL)

`user_createhome` - Whether or not to create a home directory for the user (Boolean; DEFAULT: True)

`user_homedir` - The path for the users home directory (OPTIONAL)

`user_issystem` - Whether or not the user is a system user (Boolean; DEFAULT: False)

`user_removefiles` - Used with absent state; Whether or not to also remove the user's data (Boolean; DEFAULT: False)

### Group Variables

`user_groups` - List of groups the user should belong in (OPTIONAL)

`user_groups_create` - List of dictionaries of groups which this module should either create, or ensure exist on the host (OPTIONAL)

`user_groups_remove` - List of groups to remove (OPTIONAL)

`user_groups_append` - When adding user to given groups, append given groups to user's existing list, instead of the default set to only given list (OPTIONAL)

### SSH Key Variables

`user_keys_manage` - Whether or not to manage SSH keys for the user (Boolean; DEFAULT: False)

`user_keys_list` - The list of SSH public keys to use (OPTIONAL)

### Sudo Variables

`user_sudo_allowed` - Whether or not the user should have sudo access (Boolean; DEFAULT: False)

`user_sudo_norequiretty` - Whether or not you want to restrict a TTY for sudo commands (Boolean; DEFAULT: False)

`user_sudo_nopasswd` - Whether or not you want to the user to run sudo commands without providing their password (Boolean; DEFAULT: False)

Examples
--------

#### I just want a user

```yaml
- role: user
  user_username: 'bdobalina'
```

#### My user needs sudo

```yaml
- role: user
  user_username: 'bdobalina'
  user_sudo_allowed: True
```

#### Just kidding, I still want the user but remove sudo

```yaml
- role: user
  user_username: 'bdobalina'
  user_sudo_allowed: False
```

#### Bob wants his SSH key deployed too

```yaml
- role: user
  user_username: 'bdobalina'
  user_keys_manage: True
  user_keys_list:
    - 'ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEA6NF8iallvQVp22WDkTkyrtvp9eWW6A8YVr+kz4TjGYe7gHzIw+niNltGEFHzD8+v1I2YJ6oXevct1YeS0o9HZyN1Q9qgCgzUFtdOKLv6IedplqoPkcmF0aYet2PkEDo3MlTBckFXPITAMzF8dJSIFo9D8HfdOV0IAdx4O7PtixWKn5y2hMNG0zQPyUecp4pzC6kivAIhyfHilFR61RGL+GPXQ2MWZWFYbAGjyiYJnAmCP3NOTd0jMZEnDkbUvxhMmBYSdETk1rRgm+R4LOzFUGaHqHDLKLX+FIPKcF96hrucXzcWyLbIbEgE98OHlnVYCzRdK8jlqm8tehUc9c9WhQ== vagrant insecure public key'
```

#### Bob needs some new groups for his department
```yaml
- role: user
  user_username: 'bdobalina'
  user_groups_create: 
    - name: 'funky'
    - name: 'fresh'
      gid: 7331
  user_groups:
    - 'funky'
    - 'fresh'
```

Testing and Development
-----------------------

A Vagrant file has been included, along with an example playbook to allow for quick and easy local tests. Full build tests will be added later.

TODO
----

  * Allow 'absent' removal of SSH keys
  * Support for Ansible Galaxy
  * Code cleanup/optimization
  * Travis-CI or other tests
  * ???

License
-------

This role is released under the MIT license. See LICENSE file for copyright and details.
