# Replace module 

### Replace all instances of a particular string in a file using a back-referenced regular expression[](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/replace_module.html#ansible-builtin-replace-module-replace-all-instances-of-a-particular-string-in-a-file-using-a-back-referenced-regular-expression)



## Synopsis

- This module will replace all instances of a pattern within a file.
- It is up to the user to maintain idempotence by ensuring that the same pattern would never match any replacements made.



## Parameters

| Parameter                                                    | Comments                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| **after** string added in 2.4 of ansible.builtin             | If specified, only content after this match will be replaced/removed. Can be used in combination with `before`. Uses Python regular expressions; see https://docs.python.org/3/library/re.html. Uses DOTALL, which means the `.` special character *can match newlines*. |
| **attributes** aliases: attr string added in 2.3 of ansible.builtin | The attributes the resulting filesystem object should have. To get supported flags look at the man page for *chattr* on the target system. This string should contain the attributes in the same order as the one displayed by *lsattr*. The `=` operator is assumed as default, otherwise `+` or `-` operators need to be included in the string. |
| **backup** boolean                                           | Create a backup file including  the timestamp information so you can get the original file back if you  somehow clobbered it incorrectly. Choices: no ← (default) yes |
| **before** string added in 2.4 of ansible.builtin            | If specified, only content before this match will be replaced/removed. Can be used in combination with `after`. Uses Python regular expressions; see https://docs.python.org/3/library/re.html. Uses DOTALL, which means the `.` special character *can match newlines*. |
| **encoding** string added in 2.4 of ansible.builtin          | The character encoding for reading and writing the file. Default: “utf-8” |
| **group** string                                             | Name of the group that should own the filesystem object, as would be fed to *chown*. |
| **mode** raw                                                 | The permissions the resulting filesystem object should have. For those used to */usr/bin/chmod* remember that modes are  actually octal numbers. You must either add a leading zero so that  Ansible’s YAML parser knows it is an octal number (like `0644` or `01777`) or quote it (like `'644'` or `'1777'`) so Ansible receives a string and can do its own conversion from string into number. Giving Ansible a number without following one of these rules will end up with a decimal number which will have unexpected results. As of Ansible 1.8, the mode may be specified as a symbolic mode (for example, `u+rwx` or `u=rw,g=r,o=r`). If `mode` is not specified and the destination filesystem object **does not** exist, the default `umask` on the system will be used when setting the mode for the newly created filesystem object. If `mode` is not specified and the destination filesystem object **does** exist, the mode of the existing filesystem object will be used. Specifying `mode` is the best way to ensure filesystem objects are created with the correct permissions. See CVE-2020-1736 for further details. |
| **others** string                                            | All arguments accepted by the [ansible.builtin.file](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/file_module.html#ansible-collections-ansible-builtin-file-module) module also work here. |
| **owner** string                                             | Name of the user that should own the filesystem object, as would be fed to *chown*. |
| **path** aliases: dest, destfile, name path / required       | The file to modify. Before Ansible 2.3 this option was only usable as *dest*, *destfile* and *name*. |
| **regexp** string / required                                 | The regular expression to look for in the contents of the file. Uses Python regular expressions; see https://docs.python.org/3/library/re.html. Uses MULTILINE mode, which means `^` and `$` match the beginning and end of the file, as well as the beginning and end respectively of *each line* of the file. Does not use DOTALL, which means the `.` special character matches any character *except newlines*. A common mistake is to assume that a negated character set like `[^#]` will also not match newlines. In order to exclude newlines, they must be added to the set like `[^#\n]`. Note that, as of Ansible 2.0, short form tasks should have any escape sequences backslash-escaped in order to prevent them being parsed as  string literal escapes. See the examples. |
| **replace** string                                           | The string to replace regexp matches. May contain backreferences that will get expanded with the regexp capture groups if the regexp matches. If not set, matches are removed entirely. Backreferences can be used ambiguously like `\1`, or explicitly like `\g<1>`. |
| **selevel** string                                           | The level part of the SELinux filesystem object context. This is the MLS/MCS attribute, sometimes known as the `range`. When set to `_default`, it will use the `level` portion of the policy if available. |
| **serole** string                                            | The role part of the SELinux filesystem object context. When set to `_default`, it will use the `role` portion of the policy if available. |
| **setype** string                                            | The type part of the SELinux filesystem object context. When set to `_default`, it will use the `type` portion of the policy if available. |
| **seuser** string                                            | The user part of the SELinux filesystem object context. By default it uses the `system` policy, where applicable. When set to `_default`, it will use the `user` portion of the policy if available. |
| **unsafe_writes** boolean added in 2.2 of ansible.builtin    | Influence when to use atomic operation to prevent data corruption or inconsistent reads from the target filesystem object. By default this module uses atomic operations to prevent data  corruption or inconsistent reads from the target filesystem objects, but sometimes systems are configured or just broken in ways that prevent  this. One example is docker mounted filesystem objects, which cannot be  updated atomically from inside the container and can only be written in  an unsafe manner. This option allows Ansible to fall back to unsafe methods of updating filesystem objects when atomic operations fail (however, it doesn’t  force Ansible to perform unsafe writes). IMPORTANT! Unsafe writes are subject to race conditions and can lead to data corruption. Choices: no ← (default) yes |
| **validate** string                                          | The validation command to run before copying the updated file into the final destination. A temporary file path is used to validate, passed in through ‘%s’ which must be present as in the examples below. Also, the command is passed securely so shell features such as expansion and pipes will not work. For an example on how to handle more complex validation than what this option provides, see [Complex configuration validation](https://docs.ansible.com/ansible/devel/reference_appendices/faq.html). |

## Example

```ini
- name: Before Ansible 2.3, option 'dest', 'destfile' or 'name' was used instead of 'path'
  ansible.builtin.replace:
    path: /etc/hosts
    regexp: '(\s+)old\.host\.name(\s+.*)?$'
    replace: '\1new.host.name\2'

- name: Replace after the expression till the end of the file (requires Ansible >= 2.4)
  ansible.builtin.replace:
    path: /etc/apache2/sites-available/default.conf
    after: 'NameVirtualHost [*]'
    regexp: '^(.+)$'
    replace: '# \1'

- name: Replace before the expression till the begin of the file (requires Ansible >= 2.4)
  ansible.builtin.replace:
    path: /etc/apache2/sites-available/default.conf
    before: '# live site config'
    regexp: '^(.+)$'
    replace: '# \1'

# Prior to Ansible 2.7.10, using before and after in combination did the opposite of what was intended.
# see https://github.com/ansible/ansible/issues/31354 for details.
- name: Replace between the expressions (requires Ansible >= 2.4)
  ansible.builtin.replace:
    path: /etc/hosts
    after: '<VirtualHost [*]>'
    before: '</VirtualHost>'
    regexp: '^(.+)$'
    replace: '# \1'

- name: Supports common file attributes
  ansible.builtin.replace:
    path: /home/jdoe/.ssh/known_hosts
    regexp: '^old\.host\.name[^\n]*\n'
    owner: jdoe
    group: jdoe
    mode: '0644'

- name: Supports a validate command
  ansible.builtin.replace:
    path: /etc/apache/ports
    regexp: '^(NameVirtualHost|Listen)\s+80\s*$'
    replace: '\1 127.0.0.1:8080'
    validate: '/usr/sbin/apache2ctl -f %s -t'

- name: Short form task (in ansible 2+) necessitates backslash-escaped sequences
  ansible.builtin.replace: path=/etc/hosts regexp='\\b(localhost)(\\d*)\\b' replace='\\1\\2.localdomain\\2 \\1\\2'

- name: Long form task does not
  ansible.builtin.replace:
    path: /etc/hosts
    regexp: '\b(localhost)(\d*)\b'
    replace: '\1\2.localdomain\2 \1\2'

- name: Explicitly specifying positional matched groups in replacement
  ansible.builtin.replace:
    path: /etc/ssh/sshd_config
    regexp: '^(ListenAddress[ ]+)[^\n]+$'
    replace: '\g<1>0.0.0.0'

- name: Explicitly specifying named matched groups
  ansible.builtin.replace:
    path: /etc/ssh/sshd_config
    regexp: '^(?P<dctv>ListenAddress[ ]+)(?P<host>[^\n]+)$'
    replace: '#\g<dctv>\g<host>\n\g<dctv>0.0.0.0'
```