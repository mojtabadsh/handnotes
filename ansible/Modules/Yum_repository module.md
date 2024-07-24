# yum_repository module 

### Add or remove YUM repositories

## Synopsis

- Add or remove YUM repositories in RPM-based Linux distributions.
- If you wish to update an existing repository definition use [community.general.ini_file](https://docs.ansible.com/ansible/latest/collections/community/general/ini_file_module.html#ansible-collections-community-general-ini-file-module) instead.



## Parameters

| Parameter                                                    | Comments                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| **async** boolean                                            | If set to `yes` Yum will download packages and metadata from this repo in parallel, if possible. Choices: no yes ← (default) |
| **attributes** aliases: attr string added in 2.3 of ansible.builtin | The attributes the resulting filesystem object should have. To get supported flags look at the man page for *chattr* on the target system. This string should contain the attributes in the same order as the one displayed by *lsattr*. The `=` operator is assumed as default, otherwise `+` or `-` operators need to be included in the string. |
| **bandwidth** string                                         | Maximum available network bandwidth in bytes/second. Used with the *throttle* option. If *throttle* is a percentage and bandwidth is `0` then bandwidth throttling will be disabled. If *throttle* is expressed as a data rate (bytes/sec) then this option is ignored. Default is `0` (no bandwidth throttling). Default: “0” |
| **baseurl** list / elements=string                           | URL to the directory where the yum repository’s ‘repodata’ directory lives. It can also be a list of multiple URLs. This, the *metalink* or *mirrorlist* parameters are required if *state* is set to `present`. |
| **cost** string                                              | Relative cost of accessing this repository. Useful for weighing one repo’s packages as greater/less than any other. Default: “1000” |
| **deltarpm_metadata_percentage** string                      | When the relative size of  deltarpm metadata vs pkgs is larger than this, deltarpm metadata is not  downloaded from the repo. Note that you can give values over `100`, so `200` means that the metadata is required to be half the size of the packages. Use `0` to turn off this check, and always download metadata. Default: “100” |
| **deltarpm_percentage** string                               | When the relative size of delta vs pkg is larger than this, delta is not used. Use `0` to turn off delta rpm processing. Local repositories (with [file://](file://) *baseurl*) have delta rpms turned off by default. Default: “75” |
| **description** string                                       | A human readable string describing the repository. This option corresponds to the “name” property in the repo file. This parameter is only required if *state* is set to `present`. |
| **enabled** boolean                                          | This tells yum whether or not use this repository. Yum default value is `true`. Choices: no yes |
| **enablegroups** boolean                                     | Determines whether yum will allow the use of package groups for this repository. Yum default value is `true`. Choices: no yes |
| **exclude** list / elements=string                           | List of packages to exclude from updates or installs. This should be a space separated list. Shell globs using wildcards (eg. `*` and `?`) are allowed. The list can also be a regular YAML array. |
| **failovermethod** string                                    | `roundrobin` randomly selects a URL out of the list of URLs to start with and  proceeds through each of them as it encounters a failure contacting the  host. `priority` starts from the first *baseurl* listed and reads through them sequentially. Choices: roundrobin ← (default) priority |
| **file** string                                              | File name without the `.repo` extension to save the repo in. Defaults to the value of *name*. |
| **gpgcakey** string                                          | A URL pointing to the ASCII-armored CA key file for the repository. |
| **gpgcheck** boolean                                         | Tells yum whether or not it should perform a GPG signature check on packages. No default setting. If the value is not set, the system setting from `/etc/yum.conf` or system default of `no` will be used. Choices: no yes |
| **gpgkey** list / elements=string                            | A URL pointing to the ASCII-armored GPG key file for the repository. It can also be a list of multiple URLs. |
| **group** string                                             | Name of the group that should own the filesystem object, as would be fed to *chown*. |
| **http_caching** string                                      | Determines how upstream HTTP caches are instructed to handle any HTTP downloads that Yum does. `all` means that all HTTP downloads should be cached. `packages` means that only RPM package downloads should be cached (but not repository metadata downloads). `none` means that no HTTP downloads should be cached. Choices: all ← (default) packages none |
| **include** string                                           | Include external configuration  file. Both, local path and URL is supported. Configuration file will be  inserted at the position of the *include=* line. Included files may contain further include lines. Yum will abort with an error if an inclusion loop is detected. |
| **includepkgs** list / elements=string                       | List of packages you want to  only use from a repository. This should be a space separated list. Shell globs using wildcards (eg. `*` and `?`) are allowed. Substitution variables (e.g. `$releasever`) are honored here. The list can also be a regular YAML array. |
| **ip_resolve** string                                        | Determines how yum resolves host names. `4` or `IPv4` - resolve to IPv4 addresses only. `6` or `IPv6` - resolve to IPv6 addresses only. Choices: 4 6 IPv4 IPv6 whatever ← (default) |
| **keepalive** boolean                                        | This tells yum whether or not  HTTP/1.1 keepalive should be used with this repository. This can improve transfer speeds by using one connection when downloading multiple files from a repository. Choices: no ← (default) yes |
| **keepcache** string                                         | Either `1` or `0`. Determines whether or not yum keeps the cache of headers and packages after successful installation. Choices: 0 1 ← (default) |
| **metadata_expire** string                                   | Time (in seconds) after which the metadata will expire. Default value is 6 hours. Default: “21600” |
| **metadata_expire_filter** string                            | Filter the *metadata_expire* time, allowing a trade of speed for accuracy if a command doesn’t  require it. Each yum command can specify that it requires a certain  level of timeliness quality from the remote repos. from “I’m about to  install/upgrade, so this better be current” to “Anything that’s  available is good enough”. `never` - Nothing is filtered, always obey *metadata_expire*. `read-only:past` - Commands that only care about past information are filtered from metadata expiring. Eg. *yum history* info (if history needs to lookup anything about a previous transaction, then by definition the remote package was available in the past). `read-only:present` - Commands that are balanced between past and future. Eg. *yum list yum*. `read-only:future` - Commands that are likely to result in running other commands which will require the latest metadata. Eg. *yum check-update*. Note that this option does not override “yum clean expire-cache”. Choices: never read-only:past read-only:present ← (default) read-only:future |
| **metalink** string                                          | Specifies a URL to a metalink  file for the repomd.xml, a list of mirrors for the entire repository are generated by converting the mirrors for the repomd.xml file to a *baseurl*. This, the *baseurl* or *mirrorlist* parameters are required if *state* is set to `present`. |
| **mirrorlist** string                                        | Specifies a URL to a file containing a list of baseurls. This, the *baseurl* or *metalink* parameters are required if *state* is set to `present`. |
| **mirrorlist_expire** string                                 | Time (in seconds) after which the mirrorlist locally cached will expire. Default value is 6 hours. Default: “21600” |
| **mode** raw                                                 | The permissions the resulting filesystem object should have. For those used to */usr/bin/chmod* remember that modes are  actually octal numbers. You must either add a leading zero so that  Ansible’s YAML parser knows it is an octal number (like `0644` or `01777`) or quote it (like `'644'` or `'1777'`) so Ansible receives a string and can do its own conversion from string into number. Giving Ansible a number without following one of these rules will end up with a decimal number which will have unexpected results. As of Ansible 1.8, the mode may be specified as a symbolic mode (for example, `u+rwx` or `u=rw,g=r,o=r`). If `mode` is not specified and the destination filesystem object **does not** exist, the default `umask` on the system will be used when setting the mode for the newly created filesystem object. If `mode` is not specified and the destination filesystem object **does** exist, the mode of the existing filesystem object will be used. Specifying `mode` is the best way to ensure filesystem objects are created with the correct permissions. See CVE-2020-1736 for further details. |
| **module_hotfixes** boolean added in 2.11 of ansible.builtin | Disable module RPM filtering and make all RPMs from the repository available. The default is `None`. Choices: no yes |
| **name** string / required                                   | Unique repository ID. This option builds the section name of the repository in the repo file. This parameter is only required if *state* is set to `present` or `absent`. |
| **owner** string                                             | Name of the user that should own the filesystem object, as would be fed to *chown*. |
| **password** string                                          | Password to use with the username for basic authentication.  |
| **priority** string                                          | Enforce ordered protection of repositories. The value is an integer from 1 to 99. This option only works if the YUM Priorities plugin is installed. Default: “99” |
| **protect** boolean                                          | Protect packages from updates from other repositories. Choices: no ← (default) yes |
| **proxy** string                                             | URL to the proxy server that yum should use. Set to `_none_` to disable the global proxy setting. |
| **proxy_password** string                                    | Password for this proxy.                                     |
| **proxy_username** string                                    | Username to use for proxy.                                   |
| **repo_gpgcheck** boolean                                    | This tells yum whether or not it should perform a GPG signature check on the repodata from this repository. Choices: no ← (default) yes |
| **reposdir** path                                            | Directory where the `.repo` files will be stored. Default: “/etc/yum.repos.d” |
| **retries** string                                           | Set the number of times any attempt to retrieve a file should retry before returning an error. Setting this to `0` makes yum try forever. Default: “10” |
| **s3_enabled** boolean                                       | Enables support for S3 repositories. This option only works if the YUM S3 plugin is installed. Choices: no ← (default) yes |
| **selevel** string                                           | The level part of the SELinux filesystem object context. This is the MLS/MCS attribute, sometimes known as the `range`. When set to `_default`, it will use the `level` portion of the policy if available. |
| **serole** string                                            | The role part of the SELinux filesystem object context. When set to `_default`, it will use the `role` portion of the policy if available. |
| **setype** string                                            | The type part of the SELinux filesystem object context. When set to `_default`, it will use the `type` portion of the policy if available. |
| **seuser** string                                            | The user part of the SELinux filesystem object context. By default it uses the `system` policy, where applicable. When set to `_default`, it will use the `user` portion of the policy if available. |
| **skip_if_unavailable** boolean                              | If set to `yes` yum will continue running if this repository cannot be contacted for  any reason. This should be set carefully as all repos are consulted for  any given command. Choices: no ← (default) yes |
| **ssl_check_cert_permissions** boolean                       | Whether yum should check the permissions on the paths for the certificates on the repository (both remote and local). If we can’t read any of the files then yum will force *skip_if_unavailable* to be `yes`. This is most useful for non-root processes which use yum on repos that have client cert files which are readable only by root. Choices: no ← (default) yes |
| **sslcacert** aliases: ca_cert string                        | Path to the directory containing the databases of the certificate authorities yum should use to verify SSL certificates. |
| **sslclientcert** aliases: client_cert string                | Path to the SSL client certificate yum should use to connect to repos/remote sites. |
| **sslclientkey** aliases: client_key string                  | Path to the SSL client key yum should use to connect to repos/remote sites. |
| **sslverify** aliases: validate_certs boolean                | Defines whether yum should verify SSL certificates/hosts at all. Choices: no yes ← (default) |
| **state** string                                             | State of the repo file. Choices: absent present ← (default)  |
| **throttle** string                                          | Enable bandwidth throttling for downloads. This option can be expressed as a absolute data rate in bytes/sec. An SI prefix (k, M or G) may be appended to the bandwidth value. |
| **timeout** string                                           | Number of seconds to wait for a connection before timing out. Default: “30” |
| **ui_repoid_vars** string                                    | When a repository id is displayed, append these yum variables to the string if they are used in the *baseurl*/etc. Variables are appended in the order listed (and found). Default: “releasever basearch” |
| **unsafe_writes** boolean added in 2.2 of ansible.builtin    | Influence when to use atomic operation to prevent data corruption or inconsistent reads from the target filesystem object. By default this module uses atomic operations to prevent data  corruption or inconsistent reads from the target filesystem objects, but sometimes systems are configured or just broken in ways that prevent  this. One example is docker mounted filesystem objects, which cannot be  updated atomically from inside the container and can only be written in  an unsafe manner. This option allows Ansible to fall back to unsafe methods of updating filesystem objects when atomic operations fail (however, it doesn’t  force Ansible to perform unsafe writes). IMPORTANT! Unsafe writes are subject to race conditions and can lead to data corruption. Choices: no ← (default) yes |
| **username** string                                          | Username to use for basic authentication to a repo or really any url. |



## Attributes

| Attribute      | Support        | Description                                                  |
| -------------- | -------------- | ------------------------------------------------------------ |
| **check_mode** | full           | Can run in check_mode and return changed status prediction without modifying target |
| **diff_mode**  | full           | Will return details on what has changed (or possibly needs changing in check_mode), when in diff mode |
| **platform**   | Platform: rhel | Target OS/families that can be operated against              |



## Notes

- All comments will be removed if modifying an existing repo file.
- Section order is preserved in an existing repo file.
- Parameters in a section are ordered alphabetically in an existing repo file.
- The repo file will be automatically deleted if it contains no repository.
- When removing a repository, beware that the metadata cache may still remain on disk until you run `yum clean all`. Use a notification handler for this.
- The `params` parameter was removed in Ansible 2.5 due to circumventing Ansible’s parameter handling



## Examples

```ini
- name: Add repository
  ansible.builtin.yum_repository:
    name: epel
    description: EPEL YUM repo
    baseurl: https://download.fedoraproject.org/pub/epel/$releasever/$basearch/

- name: Add multiple repositories into the same file (1/2)
  ansible.builtin.yum_repository:
    name: epel
    description: EPEL YUM repo
    file: external_repos
    baseurl: https://download.fedoraproject.org/pub/epel/$releasever/$basearch/
    gpgcheck: no

- name: Add multiple repositories into the same file (2/2)
  ansible.builtin.yum_repository:
    name: rpmforge
    description: RPMforge YUM repo
    file: external_repos
    baseurl: http://apt.sw.be/redhat/el7/en/$basearch/rpmforge
    mirrorlist: http://mirrorlist.repoforge.org/el7/mirrors-rpmforge
    enabled: no

# Handler showing how to clean yum metadata cache
- name: yum-clean-metadata
  ansible.builtin.command: yum clean metadata
  args:
    warn: no

# Example removing a repository and cleaning up metadata cache
- name: Remove repository (and clean up left-over metadata)
  ansible.builtin.yum_repository:
    name: epel
    state: absent
  notify: yum-clean-metadata

- name: Remove repository from a specific repo file
  ansible.builtin.yum_repository:
    name: epel
    file: external_repos
    state: absent
```