# Script module

### Runs a local script on a remote node after transferring it

#### add  script file in file directory or set abs path for script file

## Synopsis

- The `script` module takes the script name followed by a list of space-delimited arguments.
- Either a free form command or `cmd` parameter is required, see the examples.
- The local script at path will be transferred to the remote node and then executed.
- The given script will be processed through the shell environment on the remote node.
- This module does not require python on the remote system, much like the [ansible.builtin.raw](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/raw_module.html#ansible-collections-ansible-builtin-raw-module) module.
- This module is also supported for Windows targets.

>  Note
>
>  This module has a corresponding [action plugin](https://docs.ansible.com/ansible/latest/plugins/action.html#action-plugins).

## Parameters

| Parameter             | Comments                                                     |
| --------------------- | ------------------------------------------------------------ |
| **chdir** string      | Change into this directory on the remote node before running the script. |
| **cmd** string        | Path to the local script to run followed by optional arguments. |
| **creates** string    | A filename on the remote node, when it already exists, this step will **not** be run. |
| **decrypt** boolean   | This option controls the autodecryption of source files using vault. Choices: no yes ← (default) |
| **executable** string | Name or path of a executable to invoke the script with.      |
| **free_form** string  | Path to the local script file followed by optional arguments. |
| **removes** string    | A filename on the remote node, when it does not exist, this step will **not** be run. |

## Attributes

| Attribute      | Support                                                      | Description                                                  |
| -------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **check_mode** | partial while the script itself is arbitrary and cannot be subject to the check mode semantics it adds `creates`/`removes` options as a workaround | Can run in check_mode and return changed status prediction without modifying target |

### Note

- It is usually preferable to write Ansible modules rather than  pushing scripts. Convert your script to an Ansible module for bonus  points!
- The `ssh` connection plugin will force pseudo-tty allocation via `-tt` when scripts are executed. Pseudo-ttys do not have a stderr channel and all stderr is sent to stdout. If you depend on separated stdout and  stderr result keys, please switch to a copy+command set of tasks instead of using script.
- If the path to the local script contains spaces, it needs to be quoted.
- This module is also supported for Windows targets.



## Examples

```ini
- name: Run a script with arguments (free form)
  script: /some/local/script.sh --some-argument 1234

- name: Run a script with arguments (using 'cmd' parameter)
  script:
    cmd: /some/local/script.sh --some-argument 1234

- name: Run a script only if file.txt does not exist on the remote node
  script: /some/local/create_file.sh --some-argument 1234
  args:
    creates: /the/created/file.txt

- name: Run a script only if file.txt exists on the remote node
  ansible.builtin.script: /some/local/remove_file.sh --some-argument 1234
  args:
    removes: /the/removed/file.txt

- name: Run a script using an executable in a non-system path
  ansible.builtin.script: /some/local/script
  args:
    executable: /some/remote/executable

- name: Run a script using an executable in a system path
  ansible.builtin.script: /some/local/script.py
  args:
    executable: python3

- name: Run a Powershell script on a windows host
  script: subdirectories/under/path/with/your/playbook/script.ps1
```



