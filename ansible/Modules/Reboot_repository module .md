# Reboot module 

Reboot a machine



## Synopsis

- Reboot a machine, wait for it to go down, come back up, and respond to commands.
- For Windows targets, use the [ansible.windows.win_reboot](https://docs.ansible.com/ansible/latest/collections/ansible/windows/win_reboot_module.html#ansible-collections-ansible-windows-win-reboot-module) module instead.



> This module has a corresponding [action plugin](https://docs.ansible.com/ansible/latest/plugins/action.html#action-plugins).



## Parameters

| Parameter                                                    | Comments                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| **boot_time_command** string added in 2.10 of ansible.builtin | Command to run that returns a unique string indicating the last time the system was booted. Setting this to a command that has different output each time it is run will cause the task to fail. Default: “cat /proc/sys/kernel/random/boot_id” |
| **connect_timeout** integer                                  | Maximum seconds to wait for a successful connection to the managed hosts before trying again. If unspecified, the default setting for the underlying connection plugin is used. |
| **msg** string                                               | Message to display to users before reboot. Default: “Reboot initiated by Ansible” |
| **post_reboot_delay** integer                                | Seconds to wait after the reboot command was successful before attempting to validate the system rebooted successfully. This is useful if you want wait for something to settle despite your connection already working. Default: 0 |
| **pre_reboot_delay** integer                                 | Seconds to wait before reboot. Passed as a parameter to the reboot command. On Linux, macOS and OpenBSD, this is converted to minutes and rounded down. If less than 60, it will be set to 0. On Solaris and FreeBSD, this will be seconds. Default: 0 |
| **reboot_command** string added in 2.11 of ansible.builtin   | Command to run that reboots the system, including any parameters passed to the command. Can be an absolute path to the command or just the command name. If an absolute path to the command is not given, `search_paths` on the target system will be searched to find the absolute path. This will cause `pre_reboot_delay`, `post_reboot_delay`, and `msg` to be ignored. Default: “[determined based on target OS]” |
| **reboot_timeout** integer                                   | Maximum seconds to wait for machine to reboot and respond to a test command. This timeout is evaluated separately for both reboot verification and test command success so the maximum execution time for the module is  twice this amount. Default: 600 |
| **search_paths** list / elements=string added in 2.8 of ansible.builtin | Paths to search on the remote machine for the `shutdown` command. *Only* these paths will be searched for the `shutdown` command. `PATH` is ignored in the remote node when searching for the `shutdown` command. Default: [“/sbin”, “/bin”, “/usr/sbin”, “/usr/bin”, “/usr/local/sbin”] |
| **test_command** string                                      | Command to run on the rebooted host and expect success from to determine the machine is ready for further tasks. Default: “whoami” |



## Attributes

| Attribute            | Support         | Description                                                  |
| -------------------- | --------------- | ------------------------------------------------------------ |
| **action**           | full            | Indicates this has a corresponding action plugin so some parts of the options can be executed on the controller |
| **async**            | none            | Supports being used with the `async` keyword                 |
| **bypass_host_loop** | none            | Forces a ‘global’ task that does not execute per host, this bypasses per host templating and serial,  throttle and other loop considerations Conditionals will work as if `run_once` is being used, variables used will be from the first available host This action will not work normally outside of lockstep strategies |
| **check_mode**       | full            | Can run in check_mode and return changed status prediction without modifying target |
| **diff_mode**        | none            | Will return details on what has changed (or possibly needs changing in check_mode), when in diff mode |
| **platform**         | Platform: posix | Target OS/families that can be operated against              |



## Notes

`PATH` is ignored on the remote node when searching for the `shutdown` command. Use `search_paths` to specify locations to search if the default paths do not work.

## Examples

```ini
- name: Unconditionally reboot the machine with all defaults
  ansible.builtin.reboot:

- name: Reboot a slow machine that might have lots of updates to apply
  ansible.builtin.reboot:
    reboot_timeout: 3600

- name: Reboot a machine with shutdown command in unusual place
  ansible.builtin.reboot:
    search_paths:
     - '/lib/molly-guard'

- name: Reboot machine using a custom reboot command
  ansible.builtin.reboot:
    reboot_command: launchctl reboot userspace
    boot_time_command: uptime | cut -d ' ' -f 5
```