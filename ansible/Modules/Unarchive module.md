# unarchive module 

# Unpacks an archive after (optionally) copying it from the local machine



## Synopsis

- The `unarchive` module unpacks an archive. It will not unpack a compressed file that does not contain an archive.
- By default, it will copy the source file from the local system to the target before unpacking.
- Set `remote_src=yes` to unpack an archive which already exists on the target.
- If checksum validation is desired, use [ansible.builtin.get_url](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/get_url_module.html#ansible-collections-ansible-builtin-get-url-module) or [ansible.builtin.uri](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/uri_module.html#ansible-collections-ansible-builtin-uri-module) instead to fetch the file and set `remote_src=yes`.
- For Windows targets, use the [community.windows.win_unzip](https://docs.ansible.com/ansible/latest/collections/community/windows/win_unzip_module.html#ansible-collections-community-windows-win-unzip-module) module instead.



> This module has a corresponding [action plugin](https://docs.ansible.com/ansible/latest/plugins/action.html#action-plugins).

## [Parameters](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/unarchive_module.html#id2)[](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/unarchive_module.html#parameters)

| Parameter                                                    | Comments                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| **attributes** aliases: attr string added in 2.3 of ansible.builtin | The attributes the resulting filesystem object should have. To get supported flags look at the man page for *chattr* on the target system. This string should contain the attributes in the same order as the one displayed by *lsattr*. The `=` operator is assumed as default, otherwise `+` or `-` operators need to be included in the string. |
| **copy** boolean                                             | If true, the file is copied from local controller to the managed (remote) node, otherwise, the plugin  will look for src archive on the managed machine. This option has been deprecated in favor of `remote_src`. This option is mutually exclusive with `remote_src`. Choices: no yes ← (default) |
| **creates** path added in 1.6 of ansible.builtin             | If the specified absolute path (file or directory) already exists, this step will **not** be run. The specified absolute path (file or directory) must be below the base path given with `dest:`. |
| **decrypt** boolean added in 2.4 of ansible.builtin          | This option controls the autodecryption of source files using vault. Choices: no yes ← (default) |
| **dest** path / required                                     | Remote absolute path where the archive should be unpacked. The given path must exist. Base directory is not created by this module. |
| **exclude** list / elements=string added in 2.1 of ansible.builtin | List the directory and file entries that you would like to exclude from the unarchive action. Mutually exclusive with `include`. Default: [] |
| **extra_opts** list / elements=string added in 2.1 of ansible.builtin | Specify additional options by passing in an array. Each space-separated command-line option should be a new element of the array. See examples. Command-line options with multiple elements must use multiple lines in the array, one for each element. Default: “” |
| **group** string                                             | Name of the group that should own the filesystem object, as would be fed to *chown*. |
| **include** list / elements=string added in 2.11 of ansible.builtin | List of directory and file entries that you would like to extract from the archive. If `include` is not empty, only files listed here will be extracted. Mutually exclusive with `exclude`. Default: [] |
| **io_buffer_size** integer added in 2.12 of ansible.builtin  | Size of the volatile memory buffer that is used for extracting files from the archive in bytes. Default: 65536 |
| **keep_newer** boolean added in 2.1 of ansible.builtin       | Do not replace existing files that are newer than files from the archive. Choices: no ← (default) yes |
| **list_files** boolean added in 2.0 of ansible.builtin       | If set to True, return the list of files that are contained in the tarball. Choices: no ← (default) yes |
| **mode** raw                                                 | The permissions the resulting filesystem object should have. For those used to */usr/bin/chmod* remember that modes are  actually octal numbers. You must either add a leading zero so that  Ansible’s YAML parser knows it is an octal number (like `0644` or `01777`) or quote it (like `'644'` or `'1777'`) so Ansible receives a string and can do its own conversion from string into number. Giving Ansible a number without following one of these rules will end up with a decimal number which will have unexpected results. As of Ansible 1.8, the mode may be specified as a symbolic mode (for example, `u+rwx` or `u=rw,g=r,o=r`). If `mode` is not specified and the destination filesystem object **does not** exist, the default `umask` on the system will be used when setting the mode for the newly created filesystem object. If `mode` is not specified and the destination filesystem object **does** exist, the mode of the existing filesystem object will be used. Specifying `mode` is the best way to ensure filesystem objects are created with the correct permissions. See CVE-2020-1736 for further details. |
| **owner** string                                             | Name of the user that should own the filesystem object, as would be fed to *chown*. |
| **remote_src** boolean added in 2.2 of ansible.builtin       | Set to `yes` to indicate the archived file is already on the remote system and not local to the Ansible controller. This option is mutually exclusive with `copy`. Choices: no ← (default) yes |
| **selevel** string                                           | The level part of the SELinux filesystem object context. This is the MLS/MCS attribute, sometimes known as the `range`. When set to `_default`, it will use the `level` portion of the policy if available. |
| **serole** string                                            | The role part of the SELinux filesystem object context. When set to `_default`, it will use the `role` portion of the policy if available. |
| **setype** string                                            | The type part of the SELinux filesystem object context. When set to `_default`, it will use the `type` portion of the policy if available. |
| **seuser** string                                            | The user part of the SELinux filesystem object context. By default it uses the `system` policy, where applicable. When set to `_default`, it will use the `user` portion of the policy if available. |
| **src** path / required                                      | If `remote_src=no` (default), local path to archive file to copy to the target server; can be absolute or relative. If `remote_src=yes`, path on the target server to existing archive file to unpack. If `remote_src=yes` and `src` contains `://`, the remote machine will download the file from the URL first.  (version_added 2.0). This is only for simple cases, for full download  support use the [ansible.builtin.get_url](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/get_url_module.html#ansible-collections-ansible-builtin-get-url-module) module. |
| **unsafe_writes** boolean added in 2.2 of ansible.builtin    | Influence when to use atomic operation to prevent data corruption or inconsistent reads from the target filesystem object. By default this module uses atomic operations to prevent data  corruption or inconsistent reads from the target filesystem objects, but sometimes systems are configured or just broken in ways that prevent  this. One example is docker mounted filesystem objects, which cannot be  updated atomically from inside the container and can only be written in  an unsafe manner. This option allows Ansible to fall back to unsafe methods of updating filesystem objects when atomic operations fail (however, it doesn’t  force Ansible to perform unsafe writes). IMPORTANT! Unsafe writes are subject to race conditions and can lead to data corruption. Choices: no ← (default) yes |
| **validate_certs** boolean added in 2.2 of ansible.builtin   | This only applies if using a https URL as the source of the file. This should only set to `no` used on personally controlled sites using self-signed certificate. Prior to 2.2 the code worked as if this was set to `yes`. Choices: no yes ← (default) |



## Notes

- Requires `zipinfo` and `gtar`/`unzip` command on target host.
- Requires `zstd` command on target host to expand *.tar.zst* files.
- Can handle *.zip* files using `unzip` as well as *.tar*, *.tar.gz*, *.tar.bz2*, *.tar.xz*, and *.tar.zst* files using `gtar`.
- Does not handle *.gz* files, *.bz2* files, *.xz*, or *.zst* files that do not contain a *.tar* archive.
- Existing files/directories in the destination which are not in  the archive are not touched. This is the same behavior as a normal  archive extraction.
- Existing files/directories in the destination which are not in  the archive are ignored for purposes of deciding if the archive should  be unpacked or not.

## Examples

```ini
- name: Extract foo.tgz into /var/lib/foo
  ansible.builtin.unarchive:
    src: foo.tgz
    dest: /var/lib/foo

- name: Unarchive a file that is already on the remote machine
  ansible.builtin.unarchive:
    src: /tmp/foo.zip
    dest: /usr/local/bin
    remote_src: yes

- name: Unarchive a file that needs to be downloaded (added in 2.0)
  ansible.builtin.unarchive:
    src: https://example.com/example.zip
    dest: /usr/local/bin
    remote_src: yes

- name: Unarchive a file with extra options
  ansible.builtin.unarchive:
    src: /tmp/foo.zip
    dest: /usr/local/bin
    extra_opts:
    - --transform
    - s/^xxx/yyy/
```

## 