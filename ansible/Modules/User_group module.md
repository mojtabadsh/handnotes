# User module 

### Manage user accounts

## Synopsis

- Manage user accounts and user attributes.
- For Windows targets, use the [ansible.windows.win_user](https://docs.ansible.com/ansible/latest/collections/ansible/windows/win_user_module.html#ansible-collections-ansible-windows-win-user-module) module instead.

## Parameters

| Parameter                                                    | Comments                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| **append** boolean                                           | If `yes`, add the user to the groups specified in `groups`. If `no`, user will only be added to the groups specified in `groups`, removing them from all other groups. Choices: no ← (default) yes |
| **authorization** string added in 2.8 of ansible.builtin     | Sets the authorization of the user. Does nothing when used with other platforms. Can set multiple authorizations using comma separation. To delete all authorizations, use `authorization=''`. Currently supported on Illumos/Solaris. |
| **comment** string                                           | Optionally sets the description (aka *GECOS*) of user account. |
| **create_home** aliases: createhome boolean                  | Unless set to `no`, a home directory will be made for the user when the account is created or if the home directory does not exist. Changed from `createhome` to `create_home` in Ansible 2.5. Choices: no yes ← (default) |
| **expires** float added in 1.9 of ansible.builtin            | An expiry time for the user in epoch, it will be ignored on platforms that do not support this. Currently supported on GNU/Linux, FreeBSD, and DragonFlyBSD. Since Ansible 2.6 you can remove the expiry time by specifying a negative value. Currently supported on GNU/Linux and FreeBSD. |
| **force** boolean                                            | This only affects `state=absent`, it forces removal of the user and associated directories on supported platforms. The behavior is the same as `userdel --force`, check the man page for `userdel` on your system for details and support. When used with `generate_ssh_key=yes` this forces an existing key to be overwritten. Choices: no ← (default) yes |
| **generate_ssh_key** boolean added in 0.9 of ansible.builtin | Whether to generate a SSH key for the user in question. This will **not** overwrite an existing SSH key unless used with `force=yes`. Choices: no ← (default) yes |
| **group** string                                             | Optionally sets the user’s primary group (takes a group name). |
| **groups** list / elements=string                            | List of groups user will be added to. By default, the user is removed from all other groups. Configure `append` to modify this. When set to an empty string `''`, the user is removed from all groups except the primary group. Before Ansible 2.3, the only input format allowed was a comma separated string. |
| **hidden** boolean added in 2.6 of ansible.builtin           | macOS only, optionally hide the user from the login window and system preferences. The default will be `yes` if the *system* option is used. Choices: no yes |
| **home** path                                                | Optionally set the user’s home directory.                    |
| **local** boolean added in 2.4 of ansible.builtin            | Forces the use of “local” command alternatives on platforms that implement it. This is useful in environments that use centralized authentication  when you want to manipulate the local users (in other words, it uses `luseradd` instead of `useradd`). This will check `/etc/passwd` for an existing account before invoking commands. If the local account database exists somewhere other than `/etc/passwd`, this setting will not work properly. This requires that the above commands as well as `/etc/passwd` must exist on the target host, otherwise it will be a fatal error. Choices: no ← (default) yes |
| **login_class** string                                       | Optionally sets the user’s login class, a feature of most BSD OSs. |
| **move_home** boolean                                        | If set to `yes` when used with `home: `, attempt to move the user’s old home directory to the specified directory if it isn’t there already and the old home exists. Choices: no ← (default) yes |
| **name** aliases: user string / required                     | Name of the user to create, remove or modify.                |
| **non_unique** boolean added in 1.1 of ansible.builtin       | Optionally when used with the -u option, this option allows to change the user ID to a non-unique value. Choices: no ← (default) yes |
| **password** string                                          | Optionally set the user’s password to this crypted value. On macOS systems, this value has to be cleartext. Beware of security issues. To create a an account with a locked/disabled password on Linux systems, set this to `'!'` or `'*'`. To create a an account with a locked/disabled password on OpenBSD, set this to `'*************'`. See [FAQ entry](https://docs.ansible.com/ansible/latest/reference_appendices/faq.html#how-do-i-generate-encrypted-passwords-for-the-user-module) for details on various ways to generate these password values. |
| **password_expire_max** integer added in 2.11 of ansible.builtin | Maximum number of days between password change. Supported on Linux only. |
| **password_expire_min** integer added in 2.11 of ansible.builtin | Minimum number of days between password change. Supported on Linux only. |
| **password_lock** boolean added in 2.6 of ansible.builtin    | Lock the password (`usermod -L`, `usermod -U`, `pw lock`). Implementation differs by platform. This option does not always mean the user cannot login using other methods. This option does not disable the user, only lock the password. This must be set to `False` in order to unlock a currently locked password. The absence of this parameter will not unlock a password. Currently supported on Linux, FreeBSD, DragonFlyBSD, NetBSD, OpenBSD. Choices: no yes |
| **profile** string added in 2.8 of ansible.builtin           | Sets the profile of the user. Does nothing when used with other platforms. Can set multiple profiles using comma separation. To delete all the profiles, use `profile=''`. Currently supported on Illumos/Solaris. |
| **remove** boolean                                           | This only affects `state=absent`, it attempts to remove directories associated with the user. The behavior is the same as `userdel --remove`, check the man page for details and support. Choices: no ← (default) yes |
| **role** string added in 2.8 of ansible.builtin              | Sets the role of the user. Does nothing when used with other platforms. Can set multiple roles using comma separation. To delete all roles, use `role=''`. Currently supported on Illumos/Solaris. |
| **seuser** string added in 2.1 of ansible.builtin            | Optionally sets the seuser type (user_u) on selinux enabled systems. |
| **shell** string                                             | Optionally set the user’s shell. On macOS, before Ansible 2.5, the default shell for non-system users was `/usr/bin/false`. Since Ansible 2.5, the default shell for non-system users on macOS is `/bin/bash`. See notes for details on how other operating systems determine the default shell by the underlying tool. |
| **skeleton** string added in 2.0 of ansible.builtin          | Optionally set a home skeleton directory. Requires `create_home` option! |
| **ssh_key_bits** integer added in 0.9 of ansible.builtin     | Optionally specify number of bits in SSH key to create. The default value depends on ssh-keygen. |
| **ssh_key_comment** string added in 0.9 of ansible.builtin   | Optionally define the comment for the SSH key. Default: “ansible-generated on $HOSTNAME” |
| **ssh_key_file** path added in 0.9 of ansible.builtin        | Optionally specify the SSH key filename. If this is a relative filename then it will be relative to the user’s home directory. This parameter defaults to *.ssh/id_rsa*. |
| **ssh_key_passphrase** string added in 0.9 of ansible.builtin | Set a passphrase for the SSH key. If no passphrase is provided, the SSH key will default to having no passphrase. |
| **ssh_key_type** string added in 0.9 of ansible.builtin      | Optionally specify the type of SSH key to generate. Available SSH key types will depend on implementation present on target host. Default: “rsa” |
| **state** string                                             | Whether the account should exist or not, taking action if the state is different from what is stated. Choices: absent present ← (default) |
| **system** boolean                                           | When creating an account `state=present`, setting this to `yes` makes the user a system account. This setting cannot be changed on existing users. Choices: no ← (default) yes |
| **uid** integer                                              | Optionally sets the *UID* of the user.                       |
| **umask** string added in 2.12 of ansible.builtin            | Sets the umask of the user. Does nothing when used with other platforms. Currently supported on Linux. Requires `local` is omitted or False. |
| **update_password** string added in 1.3 of ansible.builtin   | `always` will update passwords if they differ. `on_create` will only set the password for newly created users. Choices: always ← (default) on_create |



## Attributes

| Attribute      | Support | Description                                                  |
| -------------- | ------- | ------------------------------------------------------------ |
| **check_mode** | full    | Can run in check_mode and return changed status prediction without modifying target |



## Notes

- There are specific requirements per platform on user management  utilities. However they generally come pre-installed with the system and Ansible will require they are present at runtime. If they are not, a  descriptive error message will be shown.
- On SunOS platforms, the shadow file is backed up automatically  since this module edits it directly. On other platforms, the shadow file is backed up by the underlying tools used by this module.
- On macOS, this module uses `dscl` to create, modify, and delete accounts. `dseditgroup` is used to modify group membership. Accounts are hidden from the login window by modifying `/Library/Preferences/com.apple.loginwindow.plist`.
- On FreeBSD, this module uses `pw useradd` and `chpass` to create, `pw usermod` and `chpass` to modify, `pw userdel` remove, `pw lock` to lock, and `pw unlock` to unlock accounts.
- On all other platforms, this module uses `useradd` to create, `usermod` to modify, and `userdel` to remove accounts.



## Examples

```ini
- name: Add the user 'johnd' with a specific uid and a primary group of 'admin'
  ansible.builtin.user:
    name: johnd
    comment: John Doe
    uid: 1040
    group: admin

- name: Add the user 'james' with a bash shell, appending the group 'admins' and 'developers' to the user's groups
  ansible.builtin.user:
    name: james
    shell: /bin/bash
    groups: admins,developers
    append: yes

- name: Remove the user 'johnd'
  ansible.builtin.user:
    name: johnd
    state: absent
    remove: yes

- name: Create a 2048-bit SSH key for user jsmith in ~jsmith/.ssh/id_rsa
  ansible.builtin.user:
    name: jsmith
    generate_ssh_key: yes
    ssh_key_bits: 2048
    ssh_key_file: .ssh/id_rsa

- name: Added a consultant whose account you want to expire
  ansible.builtin.user:
    name: james18
    shell: /bin/zsh
    groups: developers
    expires: 1422403387

- name: Starting at Ansible 2.6, modify user, remove expiry time
  ansible.builtin.user:
    name: james18
    expires: -1

- name: Set maximum expiration date for password
  ansible.builtin.user:
    name: ram19
    password_expire_max: 10

- name: Set minimum expiration date for password
  ansible.builtin.user:
    name: pushkar15
    password_expire_min: 5
```



# Group module – Add or remove groups

## Synopsis

- Manage presence of groups on a host.
- For Windows targets, use the [ansible.windows.win_group](https://docs.ansible.com/ansible/latest/collections/ansible/windows/win_group_module.html#ansible-collections-ansible-windows-win-group-module) module instead.



## Requirements

The below requirements are needed on the host that executes this module.

- groupadd
- groupdel
- groupmod



## Parameters

| Parameter                  | Comments                                                     |
| -------------------------- | ------------------------------------------------------------ |
| **gid** integer            | Optional *GID* to set for the group.                         |
| **local** boolean          | Forces the use of “local” command alternatives on platforms that implement it. This is useful in environments that use centralized authentication  when you want to manipulate the local groups. (for example, it uses `lgroupadd` instead of `groupadd`). This requires that these commands exist on the targeted host, otherwise it will be a fatal error. Choices: no ← (default) yes |
| **name** string / required | Name of the group to manage.                                 |
| **non_unique** boolean     | This option allows to change the group ID to a non-unique value. Requires `gid`. Not supported on macOS or BusyBox distributions. Choices: no ← (default) yes |
| **state** string           | Whether the group should be present or not on the remote host. Choices: absent present ← (default) |
| **system** boolean         | If *yes*, indicates that the group created is a system group. Choices: no ← (default) yes |



## Attributes

| Attribute      | Support | Description                                                  |
| -------------- | ------- | ------------------------------------------------------------ |
| **check_mode** | full    | Can run in check_mode and return changed status prediction without modifying target |



## Examples

```ini
- name: Ensure group "somegroup" exists
  ansible.builtin.group:
    name: somegroup
    state: present

- name: Ensure group "docker" exists with correct gid
  ansible.builtin.group:
    name: docker
    state: present
    gid: 1750
```



