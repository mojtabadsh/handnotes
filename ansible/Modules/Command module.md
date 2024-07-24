## Command module 

commands on targets

## Synopsis

- The `command` module takes the command name followed by a list of space-delimited arguments.
- The given command will be executed on all selected nodes.
- The command(s) will not be processed through the shell, so variables like `$HOSTNAME` and operations like `"*"`, `"<"`, `">"`, `"|"`, `";"` and `"&"` will not work. Use the `ansible.builtin.shell` module if you need these features.
- To create `command` tasks that are easier to read than the ones using space-delimited arguments, pass parameters using the `args` [task keyword](https://docs.ansible.com/ansible/latest/collections/ansible/reference_appendices/playbooks_keywords.html#task) or use `cmd` parameter.
- Either a free form command or `cmd` parameter is required, see the examples.
- For Windows targets, use the  `ansible.windows.win_command` module instead.

## Parameters

| Parameter                       | Comments                                                     |
| :------------------------------ | :----------------------------------------------------------- |
| **argv** list / elements=string | Passes the command as a list rather than a string. Use `argv` to avoid quoting values that would otherwise be interpreted incorrectly (for example “user name”). Only the string (free form) or the list (argv) form can be provided, not both.  One or the other must be provided. |
| **chdir**  path                 | Change into this directory before running the command.       |
| **cmd** string                  | The command to run.                                          |
| **creates** path                | A filename or (since 2.0) glob pattern. If a matching file already exists, this step **will not** be run. This is checked before *removes* is checked. |
| **free_form** string            | The command module takes a free form string as a command to run. There is no actual parameter named ‘free form’. |
| **removes** path                | A filename or (since 2.0) glob pattern. If a matching file exists, this step **will** be run. This is checked after *creates* is checked. |
| **stdin** string                | Set the stdin of the command directly to the specified value. |
| **stdin_add_newline** boolean   | If set to `yes`, append a newline to stdin data. Choices: no yes ← (default) |
| **strip_empty_ends** boolean    | Strip empty lines from the end of stdout/stderr in result. Choices: no yes ← (default) |
| **warn** boolean                | (deprecated) Enable or disable task warnings. This feature is deprecated and will be removed in 2.14. As of version 2.11, this option is now disabled by default. Choices: no ← (default) yes |

## Notes

> - If you want to run a command through the shell (say you are using `<`, `>`, `|`, and so on), you actually want the ansible.builtin.shell module instead. Parsing shell metacharacters can lead to unexpected  commands being executed if quoting is not done correctly so it is more  secure to use the `command` module when possible.
> - `creates`, `removes`, and `chdir` can be specified after the command. For instance, if you only want to run a command if a certain file does not exist, use this.
> - Check mode is supported when passing `creates` or `removes`. If running in check mode and either of these are specified, the module  will check for the existence of the file and report the correct changed  status. If these are not supplied, the task will be skipped.
> - The `executable` parameter is removed since version 2.4. If you have a need for this parameter, use the ansible.builtin.shell module instead.
> - For Windows targets, use the ansible.windows.win_command module instead.
> - For rebooting systems, use the ansible.builtin.reboot or ansible.windows.win_reboot module.

## Examples

```ini
# 'cmd' is module parameter
- name: Run command if /path/to/database does not exist (with 'cmd' parameter)
  command:
    cmd: /usr/bin/make_database.sh db_user db_name
    creates: /path/to/database

- name: Change the working directory to somedir/ and run the command as db_owner if /path/to/database does not exist
  command: /usr/bin/make_database.sh db_user db_name
  become: yes
  become_user: db_owner
  args:
    chdir: somedir/
    creates: /path/to/database
```
