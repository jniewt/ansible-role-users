Ansible Role: Users
=========

This role performs a setup of user accounts for the provided users.
It attempts to:
  - Create user accounts
  - Add users to sudo
  - Set password or force users to set their password at first login
  - Install public keys
  - Block SSH password login for the users (password is still needed for sudo)
  - Block SSH root login
  - Remove obsolete users

Requirements
------------
No special requirements.

Tested on Ansible 2.5 or higher.

Role Variables
--------------

Available variables are listed below, along with the default values (where applicable):

##### `users`
A list of users to install. Each item is a dictionary that must contain the following keys:
  - `username`
  - `key` -- a list of file paths to public keys
  - `passwd` -- [optional] password hash, if not provided, the user will be asked at first login to set the password. See [here](http://docs.ansible.com/ansible/faq.html#how-do-i-generate-crypted-passwords-for-the-user-module) for instructions how to generate an appropriate hash.

Example:

    users:
      - { username: "user1", key: [ "/home/user1/id_rsa.pub" ] }
      - username: "user2"
        key: [ "first.pub", "second.pub" ]
        passwd: "$6$8xUk42xyW.tpdk3o$PN56lzHM4ll7pIKuMKAtfRxbU7r3yZN1TrDI0EuU/wUTz9srMds1g8PN1NcCkJIVtabw6YF3rgtU9g7camrcP1"

##### `users_obsolete`
A list of users that are obsolete and are to be removed.

##### `users_ssh_permit_root_login`
Whether to permit root login over SSH at all.

    users_ssh_permit_root_login: "no"

##### `users_ssh_password_authentication`
Whether to permit password authentication over SSH for normal users.

    users_ssh_password_authentication: "no"

Dependencies
------------

None.

Example Playbook
----------------

    - hosts: servers
      vars:
        users:
          - { username: "me", key: [ "~/.ssh/id_rsa.pub" ] }
        users_obsolete:
          - "theotherguy"
      tasks:
        - import_role:
            name: jniewt.users


License
-------

MIT / BSD

Author Information
------------------

jniewt | [jniewt@gmail.com](jniewt@gmail.com)
