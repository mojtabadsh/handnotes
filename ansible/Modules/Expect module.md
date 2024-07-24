# Expect module 

### Executes a command and responds to prompts

## Synopsis

- The `expect` module executes a command and responds to prompts.
- The given command will be executed on all selected nodes. It will not be processed through the shell, so variables like `$HOME` and operations like `"<"`, `">"`, `"|"`, and `"&"` will not work.

## Requirements

The below requirements are needed on the host that executes this module.

- python >= 2.6
- pexpect >= 3.3

> ```bash
> $ yum install python-pip
> $ pip install pexpect
> $ or 
> $ apt install python3-expect
> ```

## Parameters

| Parameter                           | Comments                                                     |
| ----------------------------------- | ------------------------------------------------------------ |
| **chdir** path                      | Change into this directory before running the command.       |
| **command** string / required       | The command module takes command to run.                     |
| **creates** path                    | A filename, when it already exists, this step will **not** be run. |
| **echo** boolean                    | Whether or not to echo out your response strings. Choices: no ← (default) yes |
| **removes** path                    | A filename, when it does not exist, this step will **not** be run. |
| **responses** dictionary / required | Mapping of expected string/regex and string to respond with. If the response is a list, successive  matches return successive responses. List functionality is new in 2.1. |
| **timeout** integer                 | Amount of time in seconds to wait for the expected strings. Use `null` to disable timeout. Default: 30 |

### Note

> - If you want to run a command through the shell (say you are using `<`, `>`, `|`, and so on), you must specify a shell in the command such as `/bin/bash -c "/path/to/something | grep else"`.
> - The question, or key, under *responses* is a python regex match. Case insensitive searches are indicated with a prefix of `?i`.
> - The `pexpect` library used by this module operates with a search window of 2000  bytes, and does not use a multiline regex match. To perform a start of  line bound match, use a pattern like `(?m)^pattern`
> - By default, if a question is encountered multiple times, its  string response will be repeated. If you need different responses for  successive question matches, instead of a string response, use a list of strings as the response. The list functionality is new in 2.1.
> - The [ansible.builtin.expect](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/expect_module.html#ansible-collections-ansible-builtin-expect-module) module is designed for simple scenarios. For more complex needs, consider the use of expect code with the [ansible.builtin.shell](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/shell_module.html#ansible-collections-ansible-builtin-shell-module) or [ansible.builtin.script](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/script_module.html#ansible-collections-ansible-builtin-script-module) modules. (An example is part of the [ansible.builtin.shell](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/shell_module.html#ansible-collections-ansible-builtin-shell-module) module documentation).

## Examples

```ini
- name: Case insensitive password string match
  expect:
    command: passwd username
    responses:
      (?i)password: "MySekretPa$$word"
  no_log: true 		 # you don't want to show passwords in your logs

- name: Generic question with multiple different responses
  expect:
    command: /path/to/custom/command
    responses:
      Question:
        - response1
        - response2
        - response3
```
