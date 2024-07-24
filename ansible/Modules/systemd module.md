# systemd module 

Manage systemd units [service module](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/service_module.html#ansible-builtin-service-module-manage-services) is old version to mange service

## Synopsis

- Controls systemd units (services, timers, and so on) on remote hosts.



## Requirements

The below requirements are needed on the host that executes this module.

- A system managed by systemd.



## Parameters

| Parameter                                                    | Comments                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| **daemon_reexec** aliases: daemon-reexec boolean added in 2.8 of ansible.builtin | Run daemon_reexec command before doing any other operations, the systemd manager will serialize the manager state. Choices: no ← (default) yes |
| **daemon_reload** aliases: daemon-reload boolean             | Run daemon-reload before doing any other operations, to make sure systemd has read any changes. When set to `true`, runs daemon-reload even if the module does not start or stop anything. Choices: no ← (default) yes |
| **enabled** boolean                                          | Whether the unit should start on boot. **At least one of state and enabled are required.** Choices: no yes |
| **force** boolean added in 2.6 of ansible.builtin            | Whether to override existing symlinks. Choices: no yes       |
| **masked** boolean                                           | Whether the unit should be masked or not, a masked unit is impossible to start. Choices: no yes |
| **name** aliases: service, unit string                       | Name of the unit. This parameter takes the name of exactly one unit to work with. When no extension is given, it is implied to a `.service` as systemd. When using in a chroot environment you always need to specify the name of the unit with the extension. For example, `crond.service`. |
| **no_block** boolean added in 2.3 of ansible.builtin         | Do not synchronously wait for  the requested operation to finish. Enqueued job will continue without  Ansible blocking on its completion. Choices: no ← (default) yes |
| **scope** string added in 2.7 of ansible.builtin             | Run systemctl within a given service manager scope, either as the default system scope `system`, the current user’s scope `user`, or the scope of all users `global`. For systemd to work with ‘user’, the executing user must have its own instance of dbus started and accessible (systemd requirement). The user dbus process is normally started during normal login, but  not during the run of Ansible tasks. Otherwise you will probably get a  ‘Failed to connect to bus: no such file or directory’ error. The user must have access, normally given via setting the `XDG_RUNTIME_DIR` variable, see example below. Choices: system ← (default) user global |
| **state** string                                             | `started`/`stopped` are idempotent actions that will not run commands unless necessary. `restarted` will always bounce the unit. `reloaded` will always reload. Choices: reloaded restarted started stopped |



## Attributes

| Attribute      | Support | Description                                                  |
| -------------- | ------- | ------------------------------------------------------------ |
| **check_mode** | full    | Can run in check_mode and return changed status prediction without modifying target |



## Note

- Since 2.4, one of the following options is required `state`, `enabled`, `masked`, `daemon_reload`, (`daemon_reexec` since 2.8), and all except `daemon_reload` and (`daemon_reexec` since 2.8) also require `name`.
- Before 2.4 you always required `name`.
- Globs are not supported in name, i.e `postgres*.service`.
- The service names might vary by specific OS/distribution



## Examples

```ini
- name: Make sure a service unit is running
  ansible.builtin.systemd:
    state: started
    name: httpd

- name: Stop service cron on debian, if running
  ansible.builtin.systemd:
    name: cron
    state: stopped

- name: Restart service cron on centos, in all cases, also issue daemon-reload to pick up config changes
  ansible.builtin.systemd:
    state: restarted
    daemon_reload: yes
    name: crond

- name: Reload service httpd, in all cases
  ansible.builtin.systemd:
    name: httpd.service
    state: reloaded

- name: Enable service httpd and ensure it is not masked
  ansible.builtin.systemd:
    name: httpd
    enabled: yes
    masked: no

- name: Enable a timer unit for dnf-automatic
  ansible.builtin.systemd:
    name: dnf-automatic.timer
    state: started
    enabled: yes

- name: Just force systemd to reread configs (2.4 and above)
  ansible.builtin.systemd:
    daemon_reload: yes

- name: Just force systemd to re-execute itself (2.8 and above)
  ansible.builtin.systemd:
    daemon_reexec: yes

- name: Run a user service when XDG_RUNTIME_DIR is not set on remote login
  ansible.builtin.systemd:
    name: myservice
    state: started
    scope: user
  environment:
    XDG_RUNTIME_DIR: "/run/user/{{ myuid }}"
```