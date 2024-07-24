# archive module 

Creates a compressed archive of one or more files or trees

## Synopsis

- Creates or extends an archive.
- The source and archive are on the remote host, and the archive *is not* copied to the local host.
- Source files can be deleted after archival by specifying *remove=True*.



## Parameters

| Parameter                                                    | Comments                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| **attributes** aliases: attr string added in 2.3 of ansible.builtin | The attributes the resulting filesystem object should have. To get supported flags look at the man page for *chattr* on the target system. This string should contain the attributes in the same order as the one displayed by *lsattr*. The `=` operator is assumed as default, otherwise `+` or `-` operators need to be included in the string. |
| **dest** path                                                | The file name of the destination archive. The parent directory must exists on the remote host. This is required when `path` refers to multiple files by either specifying a glob, a directory or multiple paths in a list. If the destination archive already exists, it will be truncated and overwritten. |
| **exclude_path** list / elements=path                        | Remote absolute path, glob, or list of paths or globs for the file or files to exclude from *path* list and glob expansion. Use *exclusion_patterns* to instead exclude files or subdirectories below any of the paths from the *path* list. Default: [] |
| **exclusion_patterns** list / elements=path added in 3.2.0 of community.general | Glob style patterns to exclude files or directories from the resulting archive. This differs from *exclude_path* which applies only to the source paths from *path*. |
| **force_archive** boolean                                    | Allows you to force the module to treat this as an archive even if only a single file is specified. By default when a single file is specified it is compressed only (not archived). Enable this if you want to use [ansible.builtin.unarchive](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/unarchive_module.html#ansible-collections-ansible-builtin-unarchive-module) on an archive of a single file created with this module. Choices: no ← (default) yes |
| **format** string                                            | The type of compression to use. Support for xz was added in Ansible 2.5. Choices: bz2 gz ← (default) tar xz zip |
| **group** string                                             | Name of the group that should own the filesystem object, as would be fed to *chown*. |
| **mode** raw                                                 | The permissions the resulting filesystem object should have. For those used to */usr/bin/chmod* remember that modes are  actually octal numbers. You must either add a leading zero so that  Ansible’s YAML parser knows it is an octal number (like `0644` or `01777`) or quote it (like `'644'` or `'1777'`) so Ansible receives a string and can do its own conversion from string into number. Giving Ansible a number without following one of these rules will end up with a decimal number which will have unexpected results. As of Ansible 1.8, the mode may be specified as a symbolic mode (for example, `u+rwx` or `u=rw,g=r,o=r`). If `mode` is not specified and the destination filesystem object **does not** exist, the default `umask` on the system will be used when setting the mode for the newly created filesystem object. If `mode` is not specified and the destination filesystem object **does** exist, the mode of the existing filesystem object will be used. Specifying `mode` is the best way to ensure filesystem objects are created with the correct permissions. See CVE-2020-1736 for further details. |
| **owner** string                                             | Name of the user that should own the filesystem object, as would be fed to *chown*. |
| **path** list / elements=path / required                     | Remote absolute path, glob, or list of paths or globs for the file or files to compress or archive. |
| **remove** boolean                                           | Remove any added source files and trees after adding to archive. Choices: no ← (default) yes |
| **selevel** string                                           | The level part of the SELinux filesystem object context. This is the MLS/MCS attribute, sometimes known as the `range`. When set to `_default`, it will use the `level` portion of the policy if available. |
| **serole** string                                            | The role part of the SELinux filesystem object context. When set to `_default`, it will use the `role` portion of the policy if available. |
| **setype** string                                            | The type part of the SELinux filesystem object context. When set to `_default`, it will use the `type` portion of the policy if available. |
| **seuser** string                                            | The user part of the SELinux filesystem object context. By default it uses the `system` policy, where applicable. When set to `_default`, it will use the `user` portion of the policy if available. |
| **unsafe_writes** boolean added in 2.2 of ansible.builtin    | Influence when to use atomic operation to prevent data corruption or inconsistent reads from the target filesystem object. By default this module uses atomic operations to prevent data  corruption or inconsistent reads from the target filesystem objects, but sometimes systems are configured or just broken in ways that prevent  this. One example is docker mounted filesystem objects, which cannot be  updated atomically from inside the container and can only be written in  an unsafe manner. This option allows Ansible to fall back to unsafe methods of updating filesystem objects when atomic operations fail (however, it doesn’t  force Ansible to perform unsafe writes). IMPORTANT! Unsafe writes are subject to race conditions and can lead to data corruption. Choices: no ← (default) yes |

### Note

- Requires tarfile, zipfile, gzip and bzip2 packages on target host.
- Requires lzma or backports.lzma if using xz format.
- Can produce *gzip*, *bzip2*, *lzma* and *zip* compressed files or archives.



## Examples

```ini
- name: Compress directory /path/to/foo/ into /path/to/foo.tgz
  community.general.archive:
    path: /path/to/foo
    dest: /path/to/foo.tgz

- name: Compress regular file /path/to/foo into /path/to/foo.gz and remove it
  community.general.archive:
    path: /path/to/foo
    remove: yes

- name: Create a zip archive of /path/to/foo
  community.general.archive:
    path: /path/to/foo
    format: zip

- name: Create a bz2 archive of multiple files, rooted at /path
  community.general.archive:
    path:
    - /path/to/foo
    - /path/wong/foo
    dest: /path/file.tar.bz2
    format: bz2

- name: Create a bz2 archive of a globbed path, while excluding specific dirnames
  community.general.archive:
    path:
    - /path/to/foo/*
    dest: /path/file.tar.bz2
    exclude_path:
    - /path/to/foo/bar
    - /path/to/foo/baz
    format: bz2

- name: Create a bz2 archive of a globbed path, while excluding a glob of dirnames
  community.general.archive:
    path:
    - /path/to/foo/*
    dest: /path/file.tar.bz2
    exclude_path:
    - /path/to/foo/ba*
    format: bz2

- name: Use gzip to compress a single archive (i.e don't archive it first with tar)
  community.general.archive:
    path: /path/to/foo/single.file
    dest: /path/file.gz
    format: gz

- name: Create a tar.gz archive of a single file.
  community.general.archive:
    path: /path/to/foo/single.file
    dest: /path/file.tar.gz
    format: gz
    force_archive: true
```