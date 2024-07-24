# Set_fact module 

Set host variable(s) and fact(s)

## Synopsis

- This action allows setting variables associated to the current host.
- These variables will be available to subsequent plays during an ansible-playbook run via the host they were set on.
- Set `cacheable` to `yes` to save variables across executions using a fact cache. Variables will  keep the set_fact precedence for the current run, but will used ‘cached  fact’ precedence for subsequent ones.
- Per the standard Ansible variable precedence rules, other types  of variables have a higher priority, so this value may be overridden.



## Parameters

| Parameter                                             | Comments                                                     |
| ----------------------------------------------------- | ------------------------------------------------------------ |
| **cacheable** boolean added in 2.4 of ansible.builtin | This boolean converts the  variable into an actual ‘fact’ which will also be added to the fact  cache. It does not enable fact caching across runs, it just means it  will work with it if already enabled. Normally this module creates ‘host level variables’ and has much  higher precedence, this option changes the nature and precedence (by 7  steps) of the variable created. https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#variable-precedence-where-should-i-put-a-variable This actually creates 2 copies of the variable, a normal ‘set_fact’  host variable with high precedence and a lower ‘ansible_fact’ one that  is available for persistance via the facts cache plugin. This creates a  possibly confusing interaction with `meta: clear_facts` as it will remove the ‘ansible_fact’ but not the host variable. Choices: no ← (default) yes |
| **key_value** string / required                       | The `set_fact` module takes `key=value` pairs or `key: value` (YAML notation) as variables to set in the playbook scope. The ‘key’ is the resulting variable name and the value is, of course, the value of  said variable. You can create multiple variables at once, by supplying multiple pairs, but do NOT mix notations. |

## [Attributes](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/set_fact_module.html#id3)[](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/set_fact_module.html#attributes)

| Attribute              | Support                                                      | Description                                                  |
| ---------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **action**             | partial While the action plugin does do some of the work it relies on the  core engine to actually create the variables, that part cannot be  overriden | Indicates this has a corresponding action plugin so some parts of the options can be executed on the controller |
| **async**              | none                                                         | Supports being used with the `async` keyword                 |
| **become**             | none                                                         | Is usable alongside become keywords                          |
| **bypass_host_loop**   | none                                                         | Forces a ‘global’ task that does not execute per host, this bypasses per host templating and serial,  throttle and other loop considerations Conditionals will work as if `run_once` is being used, variables used will be from the first available host This action will not work normally outside of lockstep strategies |
| **bypass_task_loop**   | none                                                         | These tasks ignore the `loop` and `with_` keywords           |
| **check_mode**         | full                                                         | Can run in check_mode and return changed status prediction without modifying target |
| **connection**         | none                                                         | Uses the target’s configured connection information to execute code on it |
| **core**               | partial While parts of this action are implemented in core, other parts are  still available as normal plugins and can be partially overridden | This is a ‘core engine’ feature  and is not implemented like most task actions, so it is not overridable  in any way via the plugin system. |
| **delegation**         | partial while variable assignment can be delegated to a different host the execution context is always the current inventory_hostname connection variables, if set at all, would reflect the host it would target, even if we are not connecting at all in this case | Can be used in conjunction with delegate_to and related keywords |
| **diff_mode**          | none                                                         | Will return details on what has changed (or possibly needs changing in check_mode), when in diff mode |
| **ignore_conditional** | none                                                         | The action is not subject to conditional execution so it will ignore the `when:` keyword |
| **platform**           | Platforms: all                                               | Target OS/families that can be operated against              |
| **tags**               | full                                                         | Allows for the ‘tags’ keyword to control the selection of this action for execution |
| **until**              | full                                                         | Denotes if this action objeys until/retry/poll keywords      |

## Notes

- Because of the nature of tasks, set_fact will produce ‘static’  values for a variable. Unlike normal ‘lazy’ variables, the value gets  evaluated and templated on assignment.
- Some boolean values (yes, no, true, false) will always be converted to boolean type, unless `DEFAULT_JINJA2_NATIVE` is enabled.  This is done so the `var=value` booleans, otherwise it would only be able to create strings, but it  also prevents using those values to create YAML strings. Using the  setting will restrict k=v to strings, but will allow you to specify  string or boolean in YAML.
- To create lists/arrays or dictionary/hashes use YAML notation `var: [val1, val2]`.
- Since ‘cacheable’ is now a module param, ‘cacheable’ is no longer a valid fact name.



## Examples

```ini
- name: Setting host facts using key=value pairs, this format can only create strings or booleans
  ansible.builtin.set_fact: one_fact="something" other_fact="{{ local_var }}"

- name: Setting host facts using complex arguments
  ansible.builtin.set_fact:
    one_fact: something
    other_fact: "{{ local_var * 2 }}"
    another_fact: "{{ some_registered_var.results | map(attribute='ansible_facts.some_fact') | list }}"

- name: Setting facts so that they will be persisted in the fact cache
  ansible.builtin.set_fact:
    one_fact: something
    other_fact: "{{ local_var * 2 }}"
    cacheable: yes

- name: Creating list and dictionary variables
  ansible.builtin.set_fact:
    one_dict:
        something: here
        other: there
    one_list:
        - a
        - b
        - c
# As of Ansible 1.8, Ansible will convert boolean strings ('true', 'false', 'yes', 'no')
# to proper boolean values when using the key=value syntax, however it is still
# recommended that booleans be set using the complex argument style:
- name: Setting booleans using complex argument style
  ansible.builtin.set_fact:
    one_fact: yes
    other_fact: no

- name: Creating list and dictionary variables using 'shorthand' YAML
  ansible.builtin.set_fact:
    two_dict: {'something': here2, 'other': somewhere}
    two_list: [1,2,3]
    

```



**Example: Install apache on Red-hat  version**

```ini
- set_fact: pkg_name=httpd
  when: ansible_os_family=="Redhat"

- set_fact: pkg_name=apache2
  when: ansible_os_family=="suse"

- debug:
    msg: "{{pkg_name}}"

- name: Install {{pkg_name}} package
  yum:
    name: "{{pkg_name}}"
    state: present
  when: ansible_os_family=="Redhat"
```

