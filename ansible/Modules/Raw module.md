# Raw module 

### Executes a low-down and dirty command

## Synopsis

- Executes a low-down and dirty SSH command, not going through the module subsystem.
- This is useful and should only be done in a few cases. A common case is installing `python` on a system without python installed by default. Another is speaking to any devices such as routers that do not have any Python installed. In  any other case, using the [ansible.builtin.shell](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/shell_module.html#ansible-collections-ansible-builtin-shell-module) or [ansible.builtin.command](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/command_module.html#ansible-collections-ansible-builtin-command-module) module is much more appropriate.
- Arguments given to `raw` are run directly through the configured remote shell.
- Standard output, error output and return code are returned when available.
- There is no change handler support for this module.
- This module does not require python on the remote system, much like the [ansible.builtin.script](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/script_module.html#ansible-collections-ansible-builtin-script-module) module.
- This module is also supported for Windows targets.



> This module has a corresponding [action plugin](https://docs.ansible.com/ansible/latest/plugins/action.html#action-plugins).



## Parameters

| Parameter                                             | Comments                                                     |
| ----------------------------------------------------- | ------------------------------------------------------------ |
| **executable** string added in 1.0 of ansible.builtin | Change the shell used to execute the command. Should be an absolute path to the executable. When using privilege escalation (`become`) a default shell will be assigned if one is not provided as privilege escalation requires a shell. |
| **free_form** string / required                       | The raw module takes a free form command to run. There is no parameter actually named ‘free form’; see the examples! |



## Notes

- If using raw from a playbook, you may need to disable fact gathering using `gather_facts: no` if you’re using `raw` to bootstrap python onto the machine.
- If you want to execute a command securely and predictably, it may be better to use the [ansible.builtin.command](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/command_module.html#ansible-collections-ansible-builtin-command-module) or [ansible.builtin.shell](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/shell_module.html#ansible-collections-ansible-builtin-shell-module) modules instead.
- The `environment` keyword does not work with raw normally, it requires a shell which means it only works if `executable` is set or using the module with privilege escalation (`become`).



## Examples

```ini
- name: Bootstrap a host without python2 installed
  ansible.builtin.raw: dnf install -y python2 python2-dnf libselinux-python

- name: Run a command that uses non-posix shell-isms (in this example /bin/sh doesn't handle redirection and wildcards together but bash does)
  ansible.builtin.raw: cat < /tmp/*txt
  args:
    executable: /bin/bash

- name: Safely use templated variables. Always use quote filter to avoid injection issues.
  ansible.builtin.raw: "{{ package_mgr|quote }} {{ pkg_flags|quote }} install {{ python|quote }}"

- name: List user accounts on a Windows system
  ansible.builtin.raw: Get-WmiObject -Class Win32_UserAccount
```