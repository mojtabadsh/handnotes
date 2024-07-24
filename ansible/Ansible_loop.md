

# Loop - Handwrite 

```ini
- name: Standard Lopp - Touch a1,a2
  file:
    path: /home/{{ item }}
    state: touch
  with_items:
    - a1
    - a2
```

Or

```ini
- name: Standard Lopp - Touch a1,a2
  file:
    path: /home/{{ item }}
    state: touch
  loop:
    - a1
    - a2
```

**Note: If use {{item}} key word alone in line,  you must use 'single quotation' Otherwise not required**.

**Example:**

```ini
- name: Install Nginx & GCC
  yum: 
    name: '{{ item }}'
    state: present
  with_items:
    - nginx
    - gcc
```

> **DEPRECATION WARNING: Instead of using a loop to supply multiple items and
>  specifying `name: "{{ item }}"`, please use `name: ['nginx', 'gcc']` and remove the loop. **

> This feature will be removed in version 2.11. 

```ini
- name: Install Nginx & GCC
  yum: 
    name: ['nginx', 'gcc']
    state: present
```

Exercise: Create a playbook to setup the following structure on your ansible server:

```ini
- hosts: localhost
  vars_prompt:

    - name: root_path
      prompt: Where is your main path?
      private: no

    - name: project_name
      prompt: What is your project Name?
      private: no

  tasks:
  
    - name: Create Project Directories
      file:
        path: "{{root_path}}/{{project_name}}/{{item}}"
        state: directory
      loop:
        - inventory
        - roles

    - name: Create Host inventory
      file:
        path: "{{root_path}}/{{project_name}}/inventory/hosts"
        state: touch

    - name: Create project yaml File
      file:
        path: "{{root_path}}/{{project_name}}/{{project_name}}.yml"
        state: touch

    - name: Create project dir in roles Dir
      file:
        path: "{{root_path}}/{{project_name}}/roles/{{project_name}}"
        state: directory

    - name: Create Roles Dir's
      file:
        path: "{{root_path}}/{{project_name}}/roles/{{project_name}}/{{item}}"
        state: directory
      loop:
        - defaults
        - files
        - meta
        - tasks
        - templates
        - vars
        - handlers

    - name: Create Main.yml file in Roles sub Dir
      file:
        path: "{{root_path}}/{{project_name}}/roles/{{project_name}}/{{item}}/main.yml"
        state: touch
      loop:
        - defaults
        - meta
        - tasks
        - vars
        - handlers
```

Output is :

![](C:\Users\m.dadashi\Pictures\plystruc.PNG)



**Example: Create Multiple User From Var's file**

```ini
> vars/main.yml
user_list: 
- mehdi
- majid
- mojtaba

- name: create multiple user
  user: 
    name: '{{ item }}'
    state: present
  loop: '{{ user_list }}'
```

**Mange Multiple Packages with different state**

```ini
- name: multiple state to install pkg
  yum: 
    name: '{{ item.name }}'
    state: '{{ item.state }}'
  with_items:
    - {name: 'nginx', state: 'latest'}
    - {name: 'gcc' , state: 'absent'}
```

# Nested Loop

```ini
- name: create multiple databases
  mysql_db:
     name: '{{ item }}'
     state: present
  loop:
     - 'clientdb'
     - 'test'

- name: create multiple mysql user
  mysql_user:
     name: '{{ item }}'
     password: '1234'
     state: present 
  loop:  
     - mojtaba
     - dadashi
      
- name: nested loop
  mysql_user:
     name: '{{ item[0] }}'  
     priv: '{{ item[1] }}.*:ALL' 
     append_privs: yes
     login_user: root
     password: '1234'
  with_nested: 
     - {'mojtaba','dadashi'}
     - {'clientdb','test'}
```

***Note: *** **```{{item[0]}}``` replace with `mojtaba` and `{{item[1]}}` replaced with `clientdb` and `test `**

**then` {{item[0]}}` replaced with `dadashi` and `{{item[1]}}` replaced with`clientdb` and `test`**



## Loop over a `dictionary`

```ini
> vars/main.yml
my_dict:
  enviroment: producttion
  app: myapp
  version: v1.0
  ...
  
- name: loop over dictionary
  debug:
    msg: "{{ item.key }} = {{ item.value }}"
  loop: "{{ my_dict | dict2items }}"
```

**we can define a dictionary with custom `key value`  as a variable. for use this variable dictionary in playbook we can write {{ item.key }}  for key such as (environment, app. version) and use value with {{ item.value }} like that (production, myapp, v1.0) . **

***Note: In loop section set variable name and pipe | to dict2items for convert dictionary to variable1***



## Loop over `host inventory`

```ini
inventory/host
[ansible]
192.168.111.72 
192.168.111.73 

[vesal]
192.168.111.51 
192.168.111.52 
192.168.111.53 
====================

- name: loop over host inventory
  debug:  
    msg: '{{ item }}' 
  loop: "{{ groups['test'] }}"
             
- name: Show all the hosts in the inventory
  ansible.builtin.debug:
    msg: "{{ item }}"
  loop: "{{ groups['all'] }}" or "{{ ansible_play_batch }}"
```

> **To loop over your inventory, or just a subset of it, you can use a regular `loop` with the `ansible_play_batch` or `groups` variables.**
>
> **in `"{{groups['test']}}"` if use `test`, loop execute by `test` groupname in inventory host file.**
>
> **For execute by all group in inventory file use `all` or `ansible_play_batch*` key word**



## Loop control

```ini
- name: pause within a loop 
  debug:  
    msg: '{{ item }}' 
  loop: "{{ groups['all'] }}"
  loop_control:
    pause: 10
```

>  To control the time (in seconds) between the execution of each item in a task loop, use the `pause` directive with `loop_control`.



### Tracking progress through a loop with `index_var`

```ini
- name: Track loop index 
  debug: 
    msg: " The {{item}} index is {{my_index}}"
  loop:
    - Mojtaba
    - dadashi
  loop_control:
    index_var: my_index
```

>  To keep track of where you are in a loop, use the `index_var` directive with `loop_control`. This directive specifies a variable name to contain the current loop index.
>
> index_var is 0 indexed.



```ini
- name: Retry a task until a certain condition is met 
  shell: cat /home/test.txt
  register: shell_out
  until: shell_out.stdout.find("ansible") != -1
  retries: 3
  delay:  10 
```

> 3 bar run mishe har bar 10 sec waita word ansible dar file peyda beshe

