# Shell module 

# Execute shell commands on targets[](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/shell_module.html#ansible-builtin-shell-module-execute-shell-commands-on-targets)

## Synopsis

- The `shell` module takes the command name followed by a list of space-delimited arguments.
- Either a free form command or `cmd` parameter is required, see the examples.
- It is almost exactly like the [ansible.builtin.command](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/command_module.html#ansible-collections-ansible-builtin-command-module) module but runs the command through a shell (`/bin/sh`) on the remote node.
- For Windows targets, use the [ansible.windows.win_shell](https://docs.ansible.com/ansible/latest/collections/ansible/windows/win_shell_module.html#ansible-collections-ansible-windows-win-shell-module) module instead.

> This module has a corresponding [action plugin](https://docs.ansible.com/ansible/latest/plugins/action.html#action-plugins).

## [Parameters](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/shell_module.html#id2)[](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/shell_module.html#parameters)

| Parameter                     | Comments                                                     |
| ----------------------------- | ------------------------------------------------------------ |
| **chdir** path                | Change into this directory before running the command.       |
| **cmd** string                | The command to run followed by optional arguments.           |
| **creates** path              | A filename, when it already exists, this step will **not** be run. |
| **executable** path           | Change the shell used to execute the command. This expects an absolute path to the executable. |
| **free_form** string          | The shell module takes a free form command to run, as a string. There is no actual parameter named ‘free form’. See the examples on how to use this module. |
| **removes** path              | A filename, when it does not exist, this step will **not** be run. |
| **stdin** string              | Set the stdin of the command directly to the specified value. |
| **stdin_add_newline** boolean | Whether to append a newline to stdin data. Choices: no yes ← (default) |
| **warn** boolean              | Whether to enable task warnings. Choices: no yes ← (default) |



## Notes

If you want to execute a command securely and predictably, it may be better to use the [ansible.builtin.command](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/command_module.html#ansible-collections-ansible-builtin-command-module) module instead. Best practices when writing playbooks will follow the trend of using [ansible.builtin.command](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/command_module.html#ansible-collections-ansible-builtin-command-module) unless the [ansible.builtin.shell](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/shell_module.html#ansible-collections-ansible-builtin-shell-module) module is explicitly required. When running ad-hoc commands, use your best judgement.

- To sanitize any variables passed to the shell module, you should use `{{ var | quote }}` instead of just `{{ var }}` to make sure they do not include evil things like semicolons.
- An alternative to using inline shell scripts with this module is to use the [ansible.builtin.script](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/script_module.html#ansible-collections-ansible-builtin-script-module) module possibly together with the [ansible.builtin.template](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/template_module.html#ansible-collections-ansible-builtin-template-module) module.
- For rebooting systems, use the [ansible.builtin.reboot](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/reboot_module.html#ansible-collections-ansible-builtin-reboot-module) or [ansible.windows.win_reboot](https://docs.ansible.com/ansible/latest/collections/ansible/windows/win_reboot_module.html#ansible-collections-ansible-windows-win-reboot-module) module.



## Examples

```ini
- name: Execute the command in remote shell; stdout goes to the specified file on the remote
  ansible.builtin.shell: somescript.sh >> somelog.txt

- name: Change the working directory to somedir/ before executing the command
  ansible.builtin.shell: somescript.sh >> somelog.txt
  args:
    chdir: somedir/

# You can also use the 'args' form to provide the options.
- name: This command will change the working directory to somedir/ and will only run when somedir/somelog.txt doesn't exist
  ansible.builtin.shell: somescript.sh >> somelog.txt
  args:
    chdir: somedir/
    creates: somelog.txt

# You can also use the 'cmd' parameter instead of free form format.
- name: This command will change the working directory to somedir/
  ansible.builtin.shell:
    cmd: ls -l | grep log
    chdir: somedir/

- name: Run a command that uses non-posix shell-isms (in this example /bin/sh doesn't handle redirection and wildcards together but bash does)
  ansible.builtin.shell: cat < /tmp/*txt
  args:
    executable: /bin/bash

- name: Run a command using a templated variable (always use quote filter to avoid injection)
  ansible.builtin.shell: cat {{ myfile|quote }}

# You can use shell to run other executables to perform actions inline
- name: Run expect to wait for a successful PXE boot via out-of-band CIMC
  ansible.builtin.shell: |
    set timeout 300
    spawn ssh admin@{{ cimc_host }}

    expect "password:"
    send "{{ cimc_password }}\n"

    expect "\n{{ cimc_name }}"
    send "connect host\n"

    expect "pxeboot.n12"
    send "\n"

    exit 0
  args:
    executable: /usr/bin/expect
  delegate_to: localhost

# Disabling warnings
- name: Using curl to connect to a host via SOCKS proxy (unsupported in uri). Ordinarily this would throw a warning
  ansible.builtin.shell: curl --socks5 localhost:9000 http://www.ansible.com
  args:
    warn: no
```