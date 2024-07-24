# wait_for module 

Waits for a condition before continuing



## Synopsis

- You can wait for a set amount of time `timeout`, this is the default if nothing is specified or just `timeout` is specified. This does not produce an error.
- Waiting for a port to become available is useful for when  services are not immediately available after their init scripts return  which is true of certain Java application servers.
- It is also useful when starting guests with the [community.libvirt.virt](https://docs.ansible.com/ansible/latest/collections/community/libvirt/virt_module.html#ansible-collections-community-libvirt-virt-module) module and needing to pause until they are ready.
- This module can also be used to wait for a regex match a string to be present in a file.
- In Ansible 1.6 and later, this module can also be used to wait for a file to be available or absent on the filesystem.
- In Ansible 1.8 and later, this module can also be used to wait  for active connections to be closed before continuing, useful if a node  is being rotated out of a load balancer pool.
- For Windows targets, use the [ansible.windows.win_wait_for](https://docs.ansible.com/ansible/latest/collections/ansible/windows/win_wait_for_module.html#ansible-collections-ansible-windows-win-wait-for-module) module instead.



## Parameters

| Parameter                                                    | Comments                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| **active_connection_states** list / elements=string added in 2.3 of ansible.builtin | The list of TCP connection states which are counted as active connections. Default: [“ESTABLISHED”, “FIN_WAIT1”, “FIN_WAIT2”, “SYN_RECV”, “SYN_SENT”, “TIME_WAIT”] |
| **connect_timeout** integer                                  | Maximum number of seconds to wait for a connection to happen before closing and retrying. Default: 5 |
| **delay** integer                                            | Number of seconds to wait before starting to poll. Default: 0 |
| **exclude_hosts** list / elements=string added in 1.8 of ansible.builtin | List of hosts or IPs to ignore when looking for active TCP connections for `drained` state. |
| **host** string                                              | A resolvable hostname or IP address to wait for. Default: “127.0.0.1” |
| **msg** string added in 2.4 of ansible.builtin               | This overrides the normal error message from a failure to meet the required conditions. |
| **path** path added in 1.4 of ansible.builtin                | Path to a file on the filesystem that must exist before continuing. `path` and `port` are mutually exclusive parameters. |
| **port** integer                                             | Port number to poll. `path` and `port` are mutually exclusive parameters. |
| **search_regex** string added in 1.4 of ansible.builtin      | Can be used to match a string in either a file or a socket connection. Defaults to a multiline regex. |
| **sleep** integer added in 2.3 of ansible.builtin            | Number of seconds to sleep between checks. Before Ansible 2.3 this was hardcoded to 1 second. Default: 1 |
| **state** string                                             | Either `present`, `started`, or `stopped`, `absent`, or `drained`. When checking a port `started` will ensure the port is open, `stopped` will check that it is closed, `drained` will check for active connections. When checking for a file or a search string `present` or `started` will ensure that the file or string is present before continuing, `absent` will check that file is absent or removed. Choices: absent drained present started ← (default) stopped |
| **timeout** integer                                          | Maximum number of seconds to wait for, when used with another condition it will force an error. When used without other conditions it is equivalent of just sleeping. Default: 300 |



## Notes

- When waiting for a path, symbolic links will be followed.  Many  other modules that manipulate files do not follow symbolic links, so  operations on the path using other modules may not work exactly as  expected.



## Examples

```ini
- name: Sleep for 300 seconds and continue with play
  ansible.builtin.wait_for:
    timeout: 300
  delegate_to: localhost

- name: Wait for port 8000 to become open on the host, don't start checking for 10 seconds
  ansible.builtin.wait_for:
    port: 8000
    delay: 10

- name: Waits for port 8000 of any IP to close active connections, don't start checking for 10 seconds
  ansible.builtin.wait_for:
    host: 0.0.0.0
    port: 8000
    delay: 10
    state: drained

- name: Wait for port 8000 of any IP to close active connections, ignoring connections for specified hosts
  ansible.builtin.wait_for:
    host: 0.0.0.0
    port: 8000
    state: drained
    exclude_hosts: 10.2.1.2,10.2.1.3

- name: Wait until the file /tmp/foo is present before continuing
  ansible.builtin.wait_for:
    path: /tmp/foo

- name: Wait until the string "completed" is in the file /tmp/foo before continuing
  ansible.builtin.wait_for:
    path: /tmp/foo
    search_regex: completed

- name: Wait until regex pattern matches in the file /tmp/foo and print the matched group
  ansible.builtin.wait_for:
    path: /tmp/foo
    search_regex: completed (?P<task>\w+)
  register: waitfor
- ansible.builtin.debug:
    msg: Completed {{ waitfor['match_groupdict']['task'] }}

- name: Wait until the lock file is removed
  ansible.builtin.wait_for:
    path: /var/lock/file.lock
    state: absent

- name: Wait until the process is finished and pid was destroyed
  ansible.builtin.wait_for:
    path: /proc/3466/status
    state: absent

- name: Output customized message when failed
  ansible.builtin.wait_for:
    path: /tmp/foo
    state: present
    msg: Timeout to find file /tmp/foo

# Do not assume the inventory_hostname is resolvable and delay 10 seconds at start
- name: Wait 300 seconds for port 22 to become open and contain "OpenSSH"
  ansible.builtin.wait_for:
    port: 22
    host: '{{ (ansible_ssh_host|default(ansible_host))|default(inventory_hostname) }}'
    search_regex: OpenSSH
    delay: 10
  connection: local

# Same as above but you normally have ansible_connection set in inventory, which overrides 'connection'
- name: Wait 300 seconds for port 22 to become open and contain "OpenSSH"
  ansible.builtin.wait_for:
    port: 22
    host: '{{ (ansible_ssh_host|default(ansible_host))|default(inventory_hostname) }}'
    search_regex: OpenSSH
    delay: 10
  vars:
    ansible_connection: local
```