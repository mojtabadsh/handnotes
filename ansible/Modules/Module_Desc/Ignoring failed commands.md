## Ignoring failed commands

By default Ansible stops executing tasks on a host when a task fails on that host. You can use `ignore_errors` to continue on in spite of the failure.

```ini
- name: Do not count this as a failure
  ansible.builtin.command: /bin/false
  ignore_errors: yes
```

The `ignore_errors` directive only works when the task is able to run and returns a value  of ‘failed’. It does not make Ansible ignore undefined variable errors,  connection failures, execution issues (for example, missing packages),  or syntax errors.



## Ignoring unreachable host errors

New in version 2.7.

You can ignore a task failure due to the host instance being ‘UNREACHABLE’ with the `ignore_unreachable` keyword. Ansible ignores the task errors, but continues to execute  future tasks against the unreachable host. For example, at the task  level:

```ini
- name: This executes, fails, and the failure is ignored
  ansible.builtin.command: /bin/true
  ignore_unreachable: yes

- name: This executes, fails, and ends the play for this host
  ansible.builtin.command: /bin/true
```

And at the playbook level:

```ini
- hosts: all
  ignore_unreachable: yes
  tasks:
  - name: This executes, fails, and the failure is ignored
    ansible.builtin.command: /bin/true

  - name: This executes, fails, and ends the play for this host
    ansible.builtin.command: /bin/true
    ignore_unreachable: no
```