# coreos-authorized_keys

This is an Ansible role that leverages the `authorized_keys` module
in order to manage SSH keys for accessing CoreOS's shared `core` user.

It handles CoreOS' idiosyncracies (*cough* undocumented mess *cough*)
correctly, by writing a user's keys to`~core/.ssh/authorized_keys.d/`
and calling `update-ssh-keys` at the end.


## Interface

The roles only takes a `group` variable as argument.

It assumes the presence of `vars/groups.yml` and `vars/users.yml`:
- each group is represented by a key in `groups.yml`, under `groups`,
  containing a list of users;
- each user is a key in `users.yml`, under `users`, containing
  an optional `key` attribute which is passed to [`authorized_keys`](
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
  - coreos-authorized_keys
```


## Credits

I would like to thank [nigelb](https://github.com/nigelbabu) for his
work on <https://github.com/nigelbabu/infra/blob/new-users-module/>,
patience and explanations  :)
