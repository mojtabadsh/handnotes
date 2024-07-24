# Ansible Variable

Variables in Ansible are how we deal with differences between systems. To understand variables you’ll also want to dig into [Conditionals](https://docs.ansible.com/ansible/2.3/playbooks_conditionals.html) and [Loops](https://docs.ansible.com/ansible/2.3/playbooks_loops.html). Useful things like the **group_by** module and the `when` conditional can also be used with variables, and to help manage differences between systems.

For best practices advice, refer to [Variables and Vaults](https://docs.ansible.com/ansible/2.3/playbooks_best_practices.html#best-practices-for-variables-and-vaults) in the *Best Practices* chapter.



## Creating valid variable names

Not all strings are valid Ansible variable names. A variable name can only include letters, numbers, and underscores. [Python keywords](https://docs.python.org/3/reference/lexical_analysis.html#keywords) or [playbook keywords](https://docs.ansible.com/ansible/latest/reference_appendices/playbooks_keywords.html#playbook-keywords) are not valid variable names. A variable name cannot begin with a number.

Variable names can begin with an underscore. In many programming  languages, variables that begin with an underscore are private. This is  not true in Ansible. Variables that begin with an underscore are treated exactly the same as any other variable. Do not rely on this convention  for privacy or security.

This table gives examples of valid and invalid variable names:

| Valid variable names | Not valid                                                    |
| -------------------- | ------------------------------------------------------------ |
| `foo`                | `*foo`, [Python keywords](https://docs.python.org/3/reference/lexical_analysis.html#keywords) such as `async` and `lambda` |
| `foo_env`            | [playbook keywords](https://docs.ansible.com/ansible/latest/reference_appendices/playbooks_keywords.html#playbook-keywords) such as `environment` |
| `foo_port`           | `foo-port`, `foo port`, `foo.port`                           |
| `foo5`, `_foo`       | `5foo`, `12`                                                 |

### Defining simple variables

You can define a simple variable using standard YAML syntax. For example:

```ini
remote_install_path: /opt/my_app_config
```

### Referencing simple variables

After you define a variable, use Jinja2 syntax to reference it.  Jinja2 variables use double curly braces. For example, the expression `My amp goes to {{ max_amp_value }}` demonstrates the most basic form of variable substitution. You can use Jinja2 syntax in playbooks. For example:

```ini
ansible.builtin.template:
  src: foo.cfg.j2
  dest: '{{ remote_install_path }}/foo.cfg'
```

## When to quote variables (a YAML gotcha)

If you start a value with `{{ foo }}`, you must quote the whole expression to create valid YAML syntax. If you do not quote the whole expression, the YAML parser cannot interpret the syntax - it might be a variable or it might be the start of a YAML  dictionary. For guidance on writing YAML, see the [YAML Syntax](https://docs.ansible.com/ansible/latest/reference_appendices/YAMLSyntax.html#yaml-syntax) documentation.

If you use a variable without quotes like this:

```ini
- hosts: app_servers
  vars:
      app_path: {{ base_path }}/22
```

You will see: `ERROR! Syntax Error while loading YAML.` If you add quotes, Ansible works correctly:

```ini
- hosts: app_servers
  vars:
       app_path: "{{ base_path }}/22"
```



## List variables

A list variable combines a variable name with multiple values. The  multiple values can be stored as an itemized list or in square brackets `[]`, separated with commas.

### Defining variables as lists

You can define variables with multiple values using YAML lists. For example:

```ini
region:
  - northeast
  - southeast
  - midwest
```

### Referencing list variables

When you use variables defined as a list (also called an array), you  can use individual, specific fields from that list. The first item in a  list is item 0, the second item is item 1. For example:

```ini
region: "{{ region[0] }}"
```

The value of this expression would be “northeast”.



## Dictionary variables

A dictionary stores the data in key-value pairs. Usually,  dictionaries are used to store related data, such as the information  contained in an ID or a user profile.

### Defining variables as key:value dictionaries

You can define more complex variables using YAML dictionaries. A YAML dictionary maps keys to values.  For example:

```ini
foo:
  field1: one
  field2: two
```

### Referencing `key:value` dictionary variables

When you use variables defined as a key:value dictionary (also called a hash), you can use individual, specific fields from that dictionary  using either bracket notation or dot notation:

```ini
foo['field1']
foo.field1
```

Both of these examples reference the same value (“one”). Bracket  notation always works. Dot notation can cause problems because some keys collide with attributes and methods of python dictionaries. Use bracket notation if you use keys which start and end with two underscores  (which are reserved for special meanings in python) or are any of the  known public attributes:

`add`, `append`, `as_integer_ratio`, `bit_length`, `capitalize`, `center`, `clear`, `conjugate`, `copy`, `count`, `decode`, `denominator`, `difference`, `difference_update`, `discard`, `encode`, `endswith`, `expandtabs`, `extend`, `find`, `format`, `fromhex`, `fromkeys`, `get`, `has_key`, `hex`, `imag`, `index`, `insert`, `intersection`, `intersection_update`, `isalnum`, `isalpha`, `isdecimal`, `isdigit`, `isdisjoint`, `is_integer`, `islower`, `isnumeric`, `isspace`, `issubset`, `issuperset`, `istitle`, `isupper`, `items`, `iteritems`, `iterkeys`, `itervalues`, `join`, `keys`, `ljust`, `lower`, `lstrip`, `numerator`, `partition`, `pop`, `popitem`, `real`, `remove`, `replace`, `reverse`, `rfind`, `rindex`, `rjust`, `rpartition`, `rsplit`, `rstrip`, `setdefault`, `sort`, `split`, `splitlines`, `startswith`, `strip`, `swapcase`, `symmetric_difference`, `symmetric_difference_update`, `title`, `translate`, `union`, `update`, `upper`, `values`, `viewitems`, `viewkeys`, `viewvalues`, `zfill`.

```ini
myvar: "Test"
person:
 - first_name: "mojtaba"
 - last_name: "dadashi"
user_list:
 - mojtaba
 - dadashi
 - majid
user_list_2: ["mojtaba","dadashi","majid"]

---
- name: print var in vars dir
  debug:
    msg: "Full name is: {{person}}"
    
- name: print var in vars dir
  debug:
    msg: "first name is: {{person.first_name}} and last name is {{person.last_name}}"

- name:  user list index -Start by index 0 
  debug:
    msg: "{{user_list[0]}}"
```

## Registering variables

You can create variables from the output of an Ansible task with the task keyword `register`. You can use registered variables in any later tasks in your play. For example:

```ini
- hosts: web_servers

  tasks:
     - name: Run a shell command and register its output as a variable
       ansible.builtin.shell: /usr/bin/foo
       register: foo_result
       ignore_errors: true

     - name: Run a shell command using output of the previous task
       ansible.builtin.shell: /usr/bin/bar
       when: foo_result.rc == 5
```

For more examples of using registered variables in conditions on later tasks, see [Conditionals](https://docs.ansible.com/ansible/latest/user_guide/playbooks_conditionals.html#playbooks-conditionals). Registered variables may be simple variables, list variables,  dictionary variables, or complex nested data structures. The  documentation for each module includes a `RETURN` section describing the return values for that module. To see the values for a particular task, run your playbook with `-v`.

Registered variables are stored in memory. You cannot cache  registered variables for use in future plays. Registered variables are  only valid on the host for the rest of the current playbook run.

Registered variables are host-level variables. When you register a  variable in a task with a loop, the registered variable contains a value for each item in the loop. The data structure placed in the variable  during the loop will contain a `results` attribute, that is a list of all responses from the module. For a more in-depth example of how this works, see the [Loops](https://docs.ansible.com/ansible/latest/user_guide/playbooks_loops.html#playbooks-loops) section on using register with a loop.



### Understanding variable precedence

Ansible does apply variable precedence, and you might have a use for  it. Here is the order of precedence from least to greatest (the last  listed variables override all other variables):

> 1. command line values (for example, `-u my_user`, these are not variables)
> 2. role defaults (defined in role/defaults/main.yml) [1](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#id13)
> 3. inventory file or script group vars [2](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#id14)
> 4. inventory group_vars/all [3](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#id15)
> 5. playbook group_vars/all [3](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#id15)
> 6. inventory group_vars/* [3](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#id15)
> 7. playbook group_vars/* [3](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#id15)
> 8. inventory file or script host vars [2](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#id14)
> 9. inventory host_vars/* [3](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#id15)
> 10. playbook host_vars/* [3](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#id15)
> 11. host facts / cached set_facts [4](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#id16)
> 12. play vars
> 13. play vars_prompt
> 14. play vars_files
> 15. role vars (defined in role/vars/main.yml)
> 16. block vars (only for tasks in block)
> 17. task vars (only for the task)
> 18. include_vars
> 19. set_facts / registered vars
> 20. role (and include_role) params
> 21. include params
> 22. extra vars (for example, `-e "user=my_user"`)(always win precedence)



## Variables Defined in a Playbook

```ini
- hosts: webservers
  vars:
    http_port: 80
```

## Passing Variables On The Command Line

In addition to `vars_prompt` and `vars_files`, it is possible to send variables over the Ansible command line.  This is particularly useful when writing a generic release playbook where you may want to pass in the version of the application to deploy:

```bash
$ ansible-playbook release.yml --extra-vars "version=1.23.45 other_variable=foo"
```

This is useful, for, among other things, setting the hosts group or the user for the playbook.

Example:

```ini
---
- hosts: '{{ hosts }}'
  remote_user: '{{ user }}'

  tasks:
     - ...
```

```bash
$ ansible-playbook release.yml --extra-vars "hosts=vipers user=starbuck"
```

As of Ansible 1.2, you can also pass in extra vars as quoted JSON, like so:

```bash
--extra-vars '{"pacman":"mrs","ghosts":["inky","pinky","clyde","sue"]}'
```

The `key=value` form is obviously simpler, but it’s there if you need it!



### Defining variables in included files and roles

You can define variables in reusable variables files and/or in  reusable roles. When you define variables in reusable variable files,  the sensitive variables are separated from playbooks. This separation  enables you to store your playbooks in a source control software and  even share the playbooks, without the risk of exposing passwords or  other sensitive and personal data. 

This example shows how you can include variables defined in an external file:

```ini
- hosts: all
  vars_files:
    - /vars/external_vars.yml # absolute path
    - var1.yml # file in roles/projectname/vars/var1.yml

  tasks:

  - name: This is just a placeholder
    ansible.builtin.command: /bin/echo foo
```

The contents of each variables file is a simple YAML dictionary. For example:

```ini
---
# in the above example, this would be vars/external_vars.yml
somevar: somevalue
password: magic
```

> The priority is to read these files from the last to the first (`var1.yml`)
>
> If the same variable exists in all files,  Read priority is `var1.yml`

### Defining variables at runtime (Highest proirity)

You can define variables when you run your playbook by passing variables at the command line using the `--extra-vars` (or `-e`) argument. You can also request user input with a `vars_prompt` (see [Interactive input: prompts](https://docs.ansible.com/ansible/latest/user_guide/playbooks_prompts.html#playbooks-prompts)). When you pass variables at the command line, use a single quoted  string, that contains one or more variables, in one of the formats  below.

#### key=value format

**Values passed in using the `key=value` syntax are interpreted as strings. Use the JSON format if you need to  pass non-string values such as Booleans, integers, floats, lists, and so on.**

```bash
$ ansible-playbook release.yml --extra-vars "version=1.23.45 other_variable=foo"
```

#### JSON string format

```bash
$ ansible-playbook release.yml --extra-vars '{"version":"1.23.45","other_variable":"foo"}'
$ ansible-playbook arcade.yml --extra-vars '{"pacman":"mrs","ghosts":["inky","pinky","clyde","sue"]}'
```

When passing variables with `--extra-vars`, you must escape quotes and other special characters appropriately for  both your markup (for example, JSON), and for your shell:

```bash
$ ansible-playbook arcade.yml --extra-vars "{\"name\":\"Conan O\'Brien\"}"
$ ansible-playbook arcade.yml --extra-vars '{"name":"Conan O'\\\''Brien"}'
$ ansible-playbook script.yml --extra-vars "{\"dialog\":\"He said \\\"I just can\'t get enough of those $ single and double-quotes"\!"\\\"\"}"
```

If you have a lot of special characters, use a JSON or YAML file containing the variable definitions.

#### vars from a JSON or YAML file

```bash
$ ansible-playbook release.yml --extra-vars "@some_file.json"
```



# Interactive input: prompts

If you want your playbook to prompt the user for certain input, add a ‘vars_prompt’ section. Prompting the user for variables lets you avoid  recording sensitive data like passwords. In addition to security,  prompts support flexibility. For example, if you use one playbook across multiple software releases, you could prompt for the particular release version.

- Hashing values supplied by `vars_prompt`
- Allowing special characters in `vars_prompt` 

Here is a most basic example:

```ini
---
- hosts: all
  vars_prompt:

    - name: username
      prompt: What is your username?
      private: no

    - name: password
      prompt: What is your password?
      private: yes # without priview
  tasks:

    - name: Print a message
      ansible.builtin.debug:
        msg: 'Logging in as {{ username }}'
```

> The user input is hidden by default but it can be made visible by setting `private: no`.

## Hashing values supplied by `vars_prompt`

You can hash the entered value so you can use it, for instance, with the user module to define a password:

```ini
vars_prompt:

  - name: my_password2
    prompt: Enter password2
    private: yes
    encrypt: sha512_crypt
    confirm: yes
    salt_size: 7
```

## Allowing special characters in `vars_prompt` values

Some special characters, such as `{` and `%` can create templating errors. If you need to accept special characters, use the `unsafe` option:

```ini
vars_prompt:
  - name: my_password_with_weird_chars
    prompt: Enter password
    unsafe: yes
    private: yes
```

## Assigning a variable to one machine: host variables

```ini
[atlanta]
host1 http_port=80 maxRequestsPerChild=808
host2 http_port=303 maxRequestsPerChild=909
```

Unique values like non-standard SSH ports work well as host  variables. You can add them to your Ansible inventory by adding the port number after the hostname with a colon:

```ini
badwolf.example.com:5309
```

Connection variables also work well as host variables:

```ini
[targets]

localhost              ansible_connection=local
other1.example.com     ansible_connection=ssh        ansible_user=myuser
other2.example.com     ansible_connection=ssh        ansible_user=myotheruser
```

### Inventory aliases

You can also define aliases in your inventory:

In INI:

```ini
jumper ansible_port=5555 ansible_host=192.0.2.50
```



## Assigning a variable to many machines: group variables

If all hosts in a group share a variable value, you can apply that variable to an entire group at once. In INI:

```ini
[atlanta]
host1
host2

[atlanta:vars]
ntp_server=ntp.atlanta.example.com
proxy=proxy.atlanta.example.com
```



### Inheriting variable values: group variables for groups of groups

You can make groups of groups using the `:children` suffix in INI or the `children:` entry in YAML. You can apply variables to these groups of groups using `:vars` or `vars:`:

In INI:

```ini
[atlanta]
host1
host2

[raleigh]
host2
host3

[southeast:children]
atlanta
raleigh

[southeast:vars]
some_server=foo.southeast.example.com
halon_system_timeout=30
self_destruct_countdown=60
escape_pods=2

[usa:children]
southeast
northeast
southwest
northwest
```



## How variables are merged

By default variables are merged/flattened to the specific host before a play is run. This keeps Ansible focused on the Host and Task, so  groups don’t really survive outside of inventory and host matching. By  default, Ansible overwrites variables including the ones defined for a  group and/or host (see [DEFAULT_HASH_BEHAVIOUR](https://docs.ansible.com/ansible/latest/reference_appendices/config.html#default-hash-behaviour)). The order/precedence is (**from lowest to highest**):

- **all group (because it is the ‘parent’ of all other groups)**
- **parent group**
- **child group**
- **host**

By default Ansible merges groups at the same parent/child level in  ASCII order, and the last group loaded overwrites the previous groups.  For example, an a_group will be merged with b_group and b_group vars  that match will overwrite the ones in a_group.

**You can change this behavior by setting the group variable `ansible_group_priority` to change the merge order for groups of the same level (after the  parent/child order is resolved). The larger the number, the later it  will be merged, giving it higher priority. This variable defaults to `1` if not set. For example:**

```ini
a_group:
  vars:
    testvar: a
    ansible_group_priority: 10
b_group:
  vars:
    testvar: b
```

In this example, if both groups have the same priority, the result would normally have been `testvar == b`, but since we are giving the `a_group` a higher priority the result will be `testvar == a`.
