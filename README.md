# coreos-authorized_keys

This is an Ansible role that leverages the `authorized_keys` module
in order to manage SSH keys for accessing CoreOS's shared `core` user.

It handles CoreOS' idiosyncracies (*cough* undocumented mess *cough*)
correctly, by writing a user's keys to`~core/.ssh/authorized_keys.d/`
and calling `update-ssh-keys` at the end.


## Interface

The roles takes 2 parameters:
- `exclusive` can be set to `yes` (defaults to `no`) to delete SSH keys
  that are not present in Ansible;
- `group` must be set to the name of the user group that is given access.

It assumes the presence of two dicts: `user_groups` and `users`:
- each group is represented by a key in `user_groups`,
  containing a list of users;
- each user is a key in `users`, containing an optional `key` attribute
  which is passed to [`authorized_keys`](
  https://docs.ansible.com/ansible/authorized_key_module.html).
  Otherwise, the key must be put under `files/keys/$user.pub`.
  Both options **cannot** be combined.

_NOTE_: This role can be used without `gather_facts` so as to speed up
	execution.


## Example

```yaml
- hosts: coreos
  gather_facts: False
  roles:
  - { role: coreos-authorized_keys, group: admins }
```


## Credits

I would like to thank [nigelb](https://github.com/nigelbabu) for his
work on <https://github.com/nigelbabu/infra/blob/new-users-module/>,
patience and explanations  :)
