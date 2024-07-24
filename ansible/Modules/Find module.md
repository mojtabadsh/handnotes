# Find module 

Return a list of files based on specific criteria



## Synopsis

- Return a list of files based on specific criteria. Multiple criteria are AND’d together.
- For Windows targets, use the [ansible.windows.win_find](https://docs.ansible.com/ansible/latest/collections/ansible/windows/win_find_module.html#ansible-collections-ansible-windows-win-find-module) module instead.

## Parameters

| Parameter                                                    | Comments                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| **age** string                                               | Select files whose age is equal to or greater than the specified time. Use a negative age to find files equal to or less than the specified time. You can choose seconds, minutes, hours, days, or weeks by specifying the first letter of any of those words (e.g., “1w”). |
| **age_stamp** string                                         | Choose the file property against which we compare age. Choices: atime ctime mtime ← (default) |
| **contains** string                                          | A regular expression or pattern which should be matched against the file content. Works only when *file_type* is `file`. |
| **depth** integer added in 2.6 of ansible.builtin            | Set the maximum number of levels to descend into. Setting recurse to `no` will override this value, which is effectively depth 1. Default is unlimited depth. |
| **excludes** aliases: exclude list / elements=string added in 2.5 of ansible.builtin | One or more (shell or regex) patterns, which type is controlled by `use_regex` option. Items whose basenames match an `excludes` pattern are culled from `patterns` matches. Multiple patterns can be specified using a list. |
| **file_type** string                                         | Type of file to select. The ‘link’ and ‘any’ choices were added in Ansible 2.3. Choices: any directory file ← (default) link |
| **follow** boolean                                           | Set this to `yes` to follow symlinks in path for systems with python 2.6+. Choices: no ← (default) yes |
| **get_checksum** boolean                                     | Set this to `yes` to retrieve a file’s SHA1 checksum. Choices: no ← (default) yes |
| **hidden** boolean                                           | Set this to `yes` to include hidden files, otherwise they will be ignored. Choices: no ← (default) yes |
| **paths** aliases: name, path list / elements=string / required | List of paths of directories to search. All paths must be fully qualified. |
| **patterns** aliases: pattern list / elements=string         | One or more (shell or regex) patterns, which type is controlled by `use_regex` option. The patterns restrict the list of files to be returned to those whose basenames match at least one of the patterns specified. Multiple  patterns can be specified using a list. The pattern is matched against the file base name, excluding the directory. When using regexen, the pattern MUST match the ENTIRE file name, not  just parts of it. So if you are looking to match all files ending in  .default, you’d need to use ‘.*.default’ as a regexp and not just  ‘.default’. This parameter expects a list, which can be either comma separated or YAML. If any of the patterns contain a comma, make sure to put them in a list to avoid splitting the patterns in undesirable ways. Defaults to ‘*’ when `use_regex=False`, or ‘.*’ when `use_regex=True`. Default: [] |
| **read_whole_file** boolean added in 2.11 of ansible.builtin | When doing a `contains` search, determines whether the whole file should be read into memory or if the regex should be applied to the file line-by-line. Setting this to `true` can have performance and memory implications for large files. This uses `re.search(`) instead of `re.match(`). Choices: no ← (default) yes |
| **recurse** boolean                                          | If target is a directory, recursively descend into the directory looking for files. Choices: no ← (default) yes |
| **size** string                                              | Select files whose size is equal to or greater than the specified size. Use a negative size to find files equal to or less than the specified size. Unqualified values are in bytes but b, k, m, g, and t can be appended to specify bytes, kilobytes, megabytes, gigabytes, and terabytes,  respectively. Size is not evaluated for directories. |
| **use_regex** boolean                                        | If `no`, the patterns are file globs (shell). If `yes`, they are python regexes. Choices: no ← (default) yes |



## Examples

```ini
- name: Recursively find /tmp files older than 2 days
  ansible.builtin.find:
    paths: /tmp
    age: 2d
    recurse: yes

- name: Recursively find /tmp files older than 4 weeks and equal or greater than 1 megabyte
  ansible.builtin.find:
    paths: /tmp
    age: 4w
    size: 1m
    recurse: yes

- name: Recursively find /var/tmp files with last access time greater than 3600 seconds
  ansible.builtin.find:
    paths: /var/tmp
    age: 3600
    age_stamp: atime
    recurse: yes

- name: Find /var/log files equal or greater than 10 megabytes ending with .old or .log.gz
  ansible.builtin.find:
    paths: /var/log
    patterns: '*.old,*.log.gz'
    size: 10m

# Note that YAML double quotes require escaping backslashes but yaml single quotes do not.
- name: Find /var/log files equal or greater than 10 megabytes ending with .old or .log.gz via regex
  ansible.builtin.find:
    paths: /var/log
    patterns: "^.*?\\.(?:old|log\\.gz)$"
    size: 10m
    use_regex: yes

- name: Find /var/log all directories, exclude nginx and mysql
  ansible.builtin.find:
    paths: /var/log
    recurse: no
    file_type: directory
    excludes: 'nginx,mysql'

# When using patterns that contain a comma, make sure they are formatted as lists to avoid splitting the pattern
- name: Use a single pattern that contains a comma formatted as a list
  ansible.builtin.find:
    paths: /var/log
    file_type: file
    use_regex: yes
    patterns: ['^_[0-9]{2,4}_.*.log$']

- name: Use multiple patterns that contain a comma formatted as a YAML list
  ansible.builtin.find:
    paths: /var/log
    file_type: file
    use_regex: yes
    patterns:
      - '^_[0-9]{2,4}_.*.log$'
      - '^[a-z]{1,5}_.*log$'
```