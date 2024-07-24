

# About Ansible

- [Ansible Documentation](https://docs.ansible.com/ansible/latest/index.html#id1)

- Ansible is an `open source`  IT `automation tool`.  It can configure systems, `deploy` software, and `orchestrate` more advanced IT tasks such as continuous  deployments or zero downtime rolling updates. 

- Ansible’s main goals are simplicity and ease-of-use.  It also has a strong focus on security and reliability. usage of` OpenSSH` for transport (with other transports and pull modes as alternatives such as WINRM,Rest)

  > WINRM is Windows Remote Management to connect windows servers

- Ansible manages machines in an `agent-less` manner.

- Ansible can easily connect with Kerberos, LDAP, and other centralized authentication management systems.

- Ansible work with python v2.7 or 3.5 or higher 
- Ansible server must be LINUX 

- َAnsible can Rollback, if per release consist a playbook
- Ansible can run in parallel mode(Depend to SSH connection).
- Ansible Works with the user you are connected  via  SSH
- Ansible describes how a group of machines should be configured or what actions should be taken on them.
- Ansible can connect to version control system (VCS) such as Git  and read the latest change list and run it.

![Ansible-In-Devops-What-Is-Ansible-Edureka](https://d1jnx9ba8s6j9r.cloudfront.net/blog/wp-content/uploads/2016/12/Ansible-In-Devops-What-Is-Ansible-Edureka.png)

## Automation Deoloyment (Pipeline)

![Automation Deoloyment](https://dt-cdn.net/wp-content/uploads/2014/12/CD.new_.arrows-600x315.png)

# Need of Ansible

It is impossible for a human to deploy application on 1000 or higher VMs quickly.

We Need a tool such as Ansible that can do this job.

## What Ansible can do?

- **Provisioning**
- **Orchestration**
- **Configuration management**
- **Security & Compliance**
- **App Deployment**

- **Building VM templates**
- **Configuring clusters**
- **Storage management**
- **Networking**
- **Software deployment**
- **User/Group Authentication/Authorization**
- **Performance Testing**
- **OS Upgrades**
- **Software Upgrades**
- **Development Labs**
- **Alerting/Monitoring**
- **Customer Support**
- **Troubleshooting**
- **Security Patching**



## Ansible Advantages

1. **Simple** 
2. **Agentless**
3. **Powerful & Flexible** 
4. **Efficient**



## Ansible’s Agentless Architecture

![](C:\Users\m.dadashi\Desktop\Doc\Ansible\image-20220622175602585.png)

## Install Ansible

Red-hat Version

```bash
$ yum install epel-release
$ yum install ansible
$ Ansible --version
ansible 2.9.10
config file = /etc/ansible/ansible.cfg
configured module search path = [u'/usr/share/my_modules']
ansible python module location = /usr/lib/python2.7/site-packages/ansible
executable location = /usr/bin/ansible
python version = 2.7.5 (default, Oct 30 2018, 23:45:53) [GCC 4.8.5 20150623
(Red Hat 4.8.5-36)]
```



> **What is From Scratch? **
>
> Predefined number of tasks  that run on a OS

# Infrastructure as Code (IAC)

IAC is automation of it operation (Build-deploy-manage) by provisioning  of code.

Prepare infrastructure with code and automate deploy and build.

For automate this process we can use a tool such as Ansible

  

### Create Public Key

```bash
$ ssh-keygen -t rsa
$ ssh-copy-id root@192.168.X.X
```

**Save your passphrase in shell** 

```bash
$ eval "$(ssh-agent -s)" ssh-add ~/.ssh/id_rsa_ansible                    
Agent pid 10682
Enter passphrase for /root/.ssh/id_rsa_ansible: 
Identity added: /root/.ssh/id_rsa_ansible (/root/.ssh/id_rsa_ansible)
```



> We Need minimum 2 VMs for run ansible command 
>
> Create  2 VMs and copy `ssh-key` on them.



## Ansible Architecture

![](C:\Users\m.dadashi\Desktop\Doc\Ansible\B06157_CH11_01.jpg)



## Ansible Ad-hoc command

An Ansible ad-hoc command uses the `/usr/bin/ansible` command-line tool to automate a single task on one or more managed nodes.

Ad-hoc commands are great for tasks you repeat rarely and they are not reusable.

```bash
$ ansible [pattern] -m [module] -a "[module options]“ host
$ ansible –m MODULE –a COMMAND HOSTS
```

```bash
$ ansible –m ping 172.20.30.40
$ ansible –m shell –a ‘ls /home’ 172.20.30.40
$ ansible –m shell –a ‘df -h’ 172.20.30.40
$ ansible –m setup 172.20.30.40
$ ansible –m shell –a ‘whoami’ 172.20.30.40
$ ansible –m setup –a ‘filter=ansible_user_id’ 172.20.30.40
$ ansible –m setup –a ‘filter=ansible_os_family’ 172.20.30.40
$ ansible –m setup –a ‘filter=ansible_os_family’ Groupname --limit 192.168.X.X
```

>  setup module gather fact data from server.
>
> in setup output there are very ENV that start with `ansible_`. these ENV use in playbook.

#### Ansible command-line options

- --module-name MODULE_NAME , -m MODULE_NAME

  module name to execute (default=command)

- --user REMOTE_USER, -u REMOTE_USER   

  connect as this user (default=None)

- -v, --verbose       					  

  verbose mode (-vv , -vvv for more, -vvvv to enable connection debugging)

- --syntax-check       				

  perform a syntax check on the playbook, but do not execute it

### Become command-line options

- --ask-become-pass, -K

  ask for privilege escalation password; does not imply become will be used. Note that this password will be used for all hosts.

- --become, -b

  run operations with become (no password implied)

- --become-method=BECOME_METHOD

  privilege escalation method to use (default=sudo), valid choices: [ sudo | su | pbrun | pfexec | doas | dzdo | ksu | runas | machinectl ]

- --become-user=BECOME_USER

  run operations as this user (default=root), does not imply –become/-b

## Ansible Config

### **Order Path:**

```
1 ANSIBLE_CONFIG (ENV VARIABLE)
2 ./ansible.cfg
3 ~/.ansible.cfg
4 /etc/ansible/ansible.cfg
```

nearly all parameters can be overridden in ansible-playbook or with command line flags. ansible will read `ANSIBLE_CONFIG`, `ansible.cfg` in the current working directory, .`ansible.cfg` in the home directory or `/etc/ansible/ansible.cfg`, whichever it finds first

### **config  using ENV**

```bash
$ export ANSIBLE_SUDO_USER=root
```

### **Important parameter in `/etc/ansible/ansible.cfg`**

#### inventory

This is the default location of the inventory file, script, or  directory that Ansible will use to determine what hosts it has available to talk to:

```ini
inventory = /etc/ansible/hosts
```

It used to be called hostfile in Ansible before 1.9

#### library

This is the default location Ansible looks to find modules:

```ini
library = /usr/share/ansible
```

Ansible can look in multiple locations if you feed it a colon separated path, and it also will look for modules in the `./library` directory alongside a playbook.

#### forks

This is the default number of parallel processes to spawn when communicating with remote hosts.  Since Ansible 1.3, the fork number is automatically limited to the number of possible hosts at runtime, so this is really a limit of how much network and CPU load you think you can handle.  Many users may set this to 50, some set it to 500 or more.  If you have a large number of hosts, higher values will make actions across all of those hosts complete faster.  The default is very very conservative:

```ini
forks = 5
```

#### remote_port

This sets the default SSH port on all of your systems, for systems that didn’t specify an alternative value in inventory. The default is the standard 22:

```ini
remote_port = 22
```

#### remote_tmp

Ansible works by transferring modules to your remote machines, running them, and then cleaning up after itself.  In some cases, you may not wish to use the default location and would like to change the path.  You can do so by altering this setting:

```ini
remote_tmp = ~/.ansible/tmp
```

#### log_path

If present and configured in ansible.cfg, Ansible will log information about executions at the designated location.  Be sure the user running Ansible has permissions on the logfile:

```ini
log_path=/var/log/ansible.log
```

#### transfer_method

Control the mechanism for transferring files (new) If set, this will override the scp_if_ssh option

```ini
transfer_method = smart
1 smart = try sftp, scp, and piped, in that order [default]
2 sftp  = use sftp to transfer files
3 scp   = use scp to transfer files
4 piped = use 'dd' over SSH to transfer files
```

#### connect_timeout

Configures the persistent connection timeout value in seconds.  This value is how long the persistent connection will remain idle before it is destroyed. If the connection doesn't receive a request before the timeout value expires, the connection is shutdown. The default value is 30 seconds.

```ini
connect_timeout = 30 
```

#### roles_path

New in version 1.4.

The roles path indicate additional directories beyond the ‘roles/’  subdirectory of a playbook project to search to find Ansible roles.  For instance, if there was a source control repository of common roles and a different repository of playbooks, you might choose to establish a convention to checkout roles in /opt/mysite/roles  like so:

```ini
roles_path = /opt/mysite/roles
```

Additional paths can be provided separated by colon characters, in the same way as other pathstrings:

```ini
roles_path = /opt/mysite/roles:/opt/othersite/roles
```

Roles will be first searched for in the playbook directory.  Should a role not be found, it will indicate all the possible paths that were searched.

#### ask_pass

This controls whether an Ansible playbook should prompt for a password by default.  The default behavior is no:

```ini
ask_pass = True
```

If using SSH keys for authentication, it’s probably not needed to change this setting.

#### ask_sudo_pass

Similar to ask_pass, this controls whether an Ansible playbook should prompt for a sudo password by default when sudoing.  The default behavior is also no:

```ini
ask_sudo_pass = True
```

Users on platforms where sudo passwords are enabled should consider changing this setting.

#### ask_vault_pass

This controls whether an Ansible playbook should prompt for the vault password by default.  The default behavior is no:

```ini
ask_vault_pass = True
```

#### command_warnings

By default since Ansible 1.8, Ansible will issue a warning when the shell or command module is used and the command appears to be similar to an existing Ansible module. For example, this can include reminders to use the ‘git’ module instead of shell commands to execute ‘git’.  Using modules when possible over arbitrary shell commands can lead to more reliable and consistent playbook runs, and also easier to maintain playbooks:

```ini
command_warnings = False
```

These warnings can be silenced by adjusting the following setting or adding warn=yes or warn=no to the end of the command line parameter string, like so:

```yaml
- name: usage of git that could be replaced with the git module
  shell: git update foo warn=yes
```

#### deprecation_warnings

Allows disabling of deprecating warnings in ansible-playbook output:

```ini
deprecation_warnings = True
```

Deprecation warnings indicate usage of legacy features that are slated for removal in a future release of Ansible.

#### system_warnings

New in version 1.6.

Allows disabling of warnings related to potential issues on the system running ansible itself (not on the managed hosts):

```ini
system_warnings = True
```

These may include warnings about 3rd party packages or other conditions that should be resolved if possible.

#### gathering

New in 1.6, the ‘gathering’ setting controls the default policy of facts gathering (variables discovered about remote systems).

The value `implicit` is the default, which means that the fact cache  will be ignored and facts will be gathered per play unless  `gather_facts: False` is set. 

The value `explicit` is the inverse, facts will not be gathered unless  directly requested in the play. 

The value `smart` means each new host that has no facts discovered will  be scanned, but if the same host is addressed in multiple plays it will  not be contacted again in the playbook run. This option can be useful for those wishing to save fact gathering time. Both ‘smart’ and ‘explicit’ will use the fact cache:

```ini
gathering = smart
```

You can specify a subset of gathered facts, via the play’s gather_facts directive, using the following option:

```ini
gather_subset = all
```

|   all    |             gather all subsets (the default)              |
| :------: | :-------------------------------------------------------: |
| network  |                   gather network facts                    |
| hardware |     gather hardware facts (longest facts to retrieve)     |
| virtual  | gather facts about virtual machines hosted on the machine |
|   ohai   |                  gather facts from ohai                   |
|  facter  |                 gather facts from facter                  |

You can combine them using a comma separated list (ex: network,virtual,facter)

You can also disable specific subsets by prepending with a ! like this:

```ini
# Don't gather hardware facts, facts from chef's ohai or puppet's facter
gather_subset = !hardware,!ohai,!facter
```

A set of basic facts are always collected no matter which additional subsets are selected.  If you want to collect the minimal amount of facts, use !all:

```ini
gather_subset = !all
```



----

## Host Inventory 

The inventory file can be in one of many formats, depending on the inventory plugins you have. The most common formats are INI and YAML. A basic INI `/etc/ansible/hosts` might look like this:

```ini
mail.example.com
192.168.X.X

[webservers]
foo.example.com
bar.example.com
192.168.C.C

[dbservers]
one.example.com
two.example.com
three.example.com
```

The headings in brackets are group names, which are used in classifying hosts and deciding what hosts you are controlling at what times and for what purpose. Group names should follow the same guidelines as [Creating valid variable names](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#valid-variable-names).

Here’s that same basic inventory file in YAML format:

```yaml
all:
  hosts:
    mail.example.com:
  children:
    webservers:
      hosts:
        foo.example.com
        bar.example.com
    dbservers:
      hosts:
        one.example.com
        two.example.com
        three.example.com
```

### Default groups

There are two default groups: `all` and `ungrouped`. The `all` group contains every host. The `ungrouped` group contains all hosts that don’t have another group aside from `all`. Every host will always belong to at least 2 groups (`all` and `ungrouped` or `all` and some other group). Though `all` and `ungrouped` are always present, they can be implicit and not appear in group listings like `group_names`.

#### Host Range

In INI:

```ini
[webservers]
www[01:50].example.com
```

In YAML:

```ini
webservers:
    hosts:
      www[01:50].example.com:
```

#### Groups of groups

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
```

> the `children` key word is defined to ansible



## Using multiple inventory sources

You can target multiple inventory sources (directories, dynamic inventory scripts or files supported by inventory plugins) at the same time by giving multiple inventory parameters from the command line or by configuring [`ANSIBLE_INVENTORY`](https://docs.ansible.com/ansible/latest/reference_appendices/config.html#envvar-ANSIBLE_INVENTORY). This can be useful when you want to target normally separate environments, like staging and production, at the same time for a specific action.

Target two sources from the command line like this:

```bash
$ ansible-playbook project.yml -i staging -i production
```

Keep in mind that if there are variable conflicts in the inventories, they are resolved according to the rules described in [How variables are merged](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html#how-we-merge) and [Variable precedence: Where should I put a variable?](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#ansible-variable-precedence). The merging order is controlled by the order of the inventory source parameters. If `[all:vars]` in staging inventory defines `myvar = 1`, but production inventory defines `myvar = 2`, the playbook will be run with `myvar = 2`. The result would be reversed if the playbook was run with `-i production -i staging`.

**Aggregating inventory sources with a directory**

You can also create an inventory by combining multiple inventory sources and source types under a directory. This can be useful for combining static and dynamic hosts and managing them as one inventory. The following inventory combines an inventory plugin source, a dynamic inventory script, and a file with static hosts:

```yaml
inventory/
  openstack.yml          # configure inventory plugin to get hosts from Openstack cloud
  dynamic-inventory.py   # add additional hosts with dynamic inventory script
  static-inventory       # add static hosts and groups
  group_vars/
    all.yml              # assign variables to all hosts
```

You can target this inventory directory simply like this:

```bash
$ ansible-playbook example.yml -i inventory
```

It can be useful to control the merging order of the inventory sources if there’s variable conflicts or group of groups dependencies to the other inventory sources. The inventories are merged in ASCII order according to the filenames so the result can be controlled by adding prefixes to the files:

```yaml
inventory/
  01-openstack.yml          # configure inventory plugin to get hosts from Openstack cloud
  02-dynamic-inventory.py   # add additional hosts with dynamic inventory script
  03-static-inventory       # add static hosts
  group_vars/
    all.yml                 # assign variables to all hosts
```

If `01-openstack.yml` defines `myvar = 1` for the group `all`, `02-dynamic-inventory.py` defines `myvar = 2`, and `03-static-inventory` defines `myvar = 3`, the playbook will be run with `myvar = 3`.

For more details on inventory plugins and dynamic inventory scripts see [Inventory plugins](https://docs.ansible.com/ansible/latest/plugins/inventory.html#inventory-plugins) and [Working with dynamic inventory](https://docs.ansible.com/ansible/latest/user_guide/intro_dynamic_inventory.html#intro-dynamic-inventory).



## Connecting to hosts: behavioral inventory parameters

As described above, setting the following variables control how Ansible interacts with remote hosts.

Host connection:

> **Note**
>
> Ansible does not expose a channel to allow communication between the  user and the ssh process to accept a password manually to decrypt an ssh key when using the ssh connection plugin (which is the default). The  use of `ssh-agent` is highly recommended.

- ansible_connection

  Connection type to the host. This can be the name of any of ansible’s connection plugins. SSH protocol types are `smart`, `ssh` or `paramiko`.  The default is smart. Non-SSH based types are described in the next section.

General for all connections:

- ansible_host

  The name of the host to connect to, if different from the alias you wish to give to it.

- ansible_port

  The connection port number, if not the default (22 for ssh)

- ansible_user

  The user name to use when connecting to the host

- ansible_password

  The password to use to authenticate to the host (never store this variable in plain text; always use a vault. See [Keep vaulted variables safely visible](https://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html#tip-for-variables-and-vaults))

....

Examples from an Ansible-INI host file:

```ini
some_host         ansible_port=2222     ansible_user=manager
aws_host          ansible_ssh_private_key_file=/home/example/.ssh/aws.pem
freebsd_host      ansible_python_interpreter=/usr/local/bin/python
ruby_module_host  ansible_ruby_interpreter=/usr/bin/ruby.1.9.3
```



### Non-SSH connection types

As stated in the previous section, Ansible executes playbooks over SSH but it is not limited to this connection type. With the host specific parameter `ansible_connection=<connector>`, the connection type can be changed. The following non-SSH based connectors are available:

**local**

This connector can be used to deploy the playbook to the control machine itself.

**docker**

This connector deploys the playbook directly into Docker containers  using the local Docker client. The following parameters are processed by this connector:

- ansible_host

  The name of the Docker container to connect to.

- ansible_user

  The user name to operate within the container. The user must exist inside the container.

- ansible_become

  If set to `true` the `become_user` will be used to operate within the container.

- ansible_docker_extra_args

  Could be a string with any  additional arguments understood by Docker, which are not command  specific. This parameter is mainly used to configure a remote Docker  daemon to use.



## Playbook

Ansible Playbooks offer a repeatable, re-usable, simple configuration  management and multi-machine deployment system, one that is well suited  to deploying complex applications. If you need to execute a task with  Ansible more than once, write a playbook and put it under source  control. Then you can use the playbook to push out new configuration or  confirm the configuration of remote systems. 

Playbooks can:

- declare configurations
- orchestrate steps of any manual ordered process, on multiple sets of machines, in a defined order
- launch tasks synchronously or asynchronously

## Playbook syntax

Playbooks are expressed in YAML format with a minimum of syntax. If you are not familiar with YAML, look at our overview of [YAML Syntax](https://docs.ansible.com/ansible/latest/reference_appendices/YAMLSyntax.html#yaml-syntax) and consider installing an add-on for your text editor (see [Other Tools and Programs](https://docs.ansible.com/ansible/latest/community/other_tools_and_programs.html#other-tools-and-programs)) to help you write clean YAML syntax in your playbooks.

A playbook is composed of one or more ‘plays’ in an ordered list. The terms ‘playbook’ and ‘play’ are sports analogies. Each play executes  part of the overall goal of the playbook, running one or more tasks.  Each task calls an Ansible module.

### Playbook keywords

Any playbook keyword will override any command-line option and any configuration setting.

Within playbook keywords, precedence flows with the playbook itself; the more specific wins against the more general:

- play (most general)
- blocks/includes/imports/roles (optional and can contain tasks and each other)
- tasks (most specific)

A simple example:

```ini
- hosts: all
  connection: ssh
  tasks:
    - name: This task uses ssh.
      ping:

    - name: This task uses paramiko.
      connection: paramiko
      ping:
```

### Playbook file Structure

```ini
setup-webserver/
├── inventory
│   └── hosts
├── roles
│   └── setup-webserver
│       ├── defaults
│       │   └── main.yml
│       ├── files
│       │   └── helloworld.war
│       ├── handlers
│       │   └── main.yml
│       ├── meta
│       │   └── main.yml
│       ├── tasks
│       │   └── main.yml
│       ├── templates
│       │   ├── context-hostmanager.xml.j2
│       │   ├── context-manager.xml.j2
│       │   ├── server.xml.j2
│       │   ├── tomcat.service.j2
│       │   └── tomcat-users.xml.j2
│       └── vars
│           └── main.yml
└── setup-webserver.yml
```

### Role directory structure

Roles expect files to be in certain directory names. Roles must  include at least one of these directories, however it is perfectly fine  to exclude any which are not being used. When in use, each directory  must contain a `main.yml` file, which contains the relevant content:

- **`tasks` - contains the main list of tasks to be executed by the role.**
- **`handlers` - contains handlers, which may be used by this role or even anywhere outside this role.(Manage services)**
- **`defaults` - default variables for the role (see [Using Variables](https://docs.ansible.com/ansible/2.9/user_guide/playbooks_variables.html#playbooks-variables) for more information).**
- **`vars` - other variables for the role (see [Using Variables](https://docs.ansible.com/ansible/2.9/user_guide/playbooks_variables.html#playbooks-variables) for more information).**
- **`files` - contains files which can be deployed via this role.**
- **`templates` - contains templates which can be deployed via this role.**
- **`meta` - defines some meta data for this role.**

### Using Roles

The classic (original) way to use roles is via the `roles:` option for a given Main project playbook

```ini
---
- hosts: webservers
  roles:
    - setup-webserver
    - ...
```



### ansible-playbook command-line options

**Runs Ansible playbooks, executing the defined tasks on the targeted hosts.**

#### [Synopsis](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html#id2)

```ini
ansible-playbook [-h] [--version] [-v] [--private-key PRIVATE_KEY_FILE]
                     [-u REMOTE_USER] [-c CONNECTION] [-T TIMEOUT]
                     [--ssh-common-args SSH_COMMON_ARGS]
                     [--sftp-extra-args SFTP_EXTRA_ARGS]
                     [--scp-extra-args SCP_EXTRA_ARGS]
                     [--ssh-extra-args SSH_EXTRA_ARGS]
                     [-k | --connection-password-file CONNECTION_PASSWORD_FILE]
                     [--force-handlers] [--flush-cache] [-b]
                     [--become-method BECOME_METHOD]
                     [--become-user BECOME_USER]
                     [-K | --become-password-file BECOME_PASSWORD_FILE]
                     [-t TAGS] [--skip-tags SKIP_TAGS] [-C]
                     [--syntax-check] [-D] [-i INVENTORY] [--list-hosts]
                     [-l SUBSET] [-e EXTRA_VARS] [--vault-id VAULT_IDS]
                     [--ask-vault-password | --vault-password-file VAULT_PASSWORD_FILES]
                     [-f FORKS] [-M MODULE_PATH] [--list-tasks]
                     [--list-tags] [--step] [--start-at-task START_AT_TASK]
                     playbook [playbook ...]
```

## Common Options

- --ask-vault-password, --ask-vault-pass

  ask for vault password

- --become-user <BECOME_USER>

  run operations as this user (default=root)

- --flush-cache

  clear the fact cache for every host in inventory

- --list-hosts

  outputs a list of matching hosts; does not execute anything else

- --list-tags

  list all available tags

- --list-tasks

  list all tasks that would be executed

- --skip-tags

  only run plays and tasks whose tags do not match these values

- --start-at-task <START_AT_TASK>

  start the playbook at the task matching this name

- --step

  one-step-at-a-time: confirm each task before running(yes/no/continue)

- --syntax-check

  perform a syntax check on the playbook, but do not execute it

- --vault-password-file, --vault-pass-file

  vault password file

- -C, --check **(Dry Run)**

  don’t make any changes; instead, try to predict some of the changes that may occur

- -D, --diff

  when changing (small) files and templates, show the differences in those files; works great with –check

- -e, --extra-vars

  set additional variables as key=value or YAML/JSON, if filename prepend with @

- -i, --inventory, --inventory-file

- specify inventory host path or comma separated host list. –inventory-file is deprecated

- -k, --ask-pass

  ask for connection password

- -t, --tags

- only run plays and tasks tagged with these values

- -v, --verbose

  Causes Ansible to print more debug messages. Adding multiple -v  will increase the verbosity, the builtin plugins currently evaluate up  to -vvvvvv. A reasonable level to start is -vvv, connection debugging  might require -vvvv.

# Check Mode (Dry Run)

When ansible-playbook is executed with `--check` it will not make any changes on remote systems.  Instead, any module instrumented to support ‘check mode’ (which contains most of the primary core modules, but it is not required that all modules do this) will report what changes they would have made rather than making  them.  Other modules that do not support check mode will also take no  action, but just will not report what changes they might have made.

Check mode is just a simulation, and if you have steps that use conditionals that depend on the results of prior commands, it may be less useful for you.  However it is great for one-node-at-time basic configuration management use cases.

Example:

```bash
$ ansible-playbook foo.yml --check
```

## Enabling or disabling check mode for tasks

New in version 2.2.

Sometimes you may want to modify the check mode behavior of individual tasks. This is done via the `check_mode` option, which can be added to tasks.

There are two options:

1. Force a task to **run in check mode**, even when the playbook is called **without** `--check`. This is called `check_mode: yes`.
2. Force a task to **run in normal mode** and make changes to the system, even when the playbook is called **with** `--check`. This is called `check_mode: no`.

>  Note
>
>  Prior to version 2.2 only the the equivalent of `check_mode: no` existed. The notation for that was `always_run: yes`.

Instead of `yes`/`no` you can use a Jinja2 expression, just like the `when` clause.

Example:

```yaml
tasks:
  - name: this task will make changes to the system even in check mode
    command: /something/to/run --even-in-check-mode
    check_mode: no

  - name: this task will always run under checkmode and not change the system
    lineinfile: line="important config" dest=/path/to/myconfig.conf state=present
    check_mode: yes
```

Running single tasks with `check_mode: yes` can be useful to write tests for ansible modules, either to test the module itself or to the the conditions under which a module would make changes. With `register` (see [Conditionals](https://docs.ansible.com/ansible/2.3/playbooks_conditionals.html)) you can check the potential changes.

## Information about check mode in variables

New in version 2.1.

If you want to skip, or ignore errors on some tasks in check mode you can use a boolean magic variable `ansible_check_mode` which will be set to `True` during check mode.

Example:

```yaml
tasks:
  - name: this task will be skipped in check mode
    git: repo=ssh://git@github.com/mylogin/hello.git dest=/home/mylogin/hello
    when: not ansible_check_mode

  - name: this task will ignore errors in check mode
    git: repo=ssh://git@github.com/mylogin/hello.git dest=/home/mylogin/hello
    ignore_errors: "{{ ansible_check_mode }}"
```



## Showing Differences with `--diff`

New in version 1.1.

The `--diff` option to ansible-playbook works great with `--check` (detailed above) but can also be used by itself.  When this flag is  supplied, if any templated files on the remote system are changed, and  the ansible-playbook CLI will report back the textual changes made to the file (or, if used with `--check`, the changes that would have been made).  Since the diff feature produces a large amount of output, it is best used when checking a single host at a time, like so:

```bash
$ ansible-playbook foo.yml --check --diff --limit foo.example.com
```
