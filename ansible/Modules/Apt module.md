# Apt module 

### Manages apt-packages



## Synopsis

- Manages *apt* packages (such as for Debian/Ubuntu).



## Requirements

The below requirements are needed on the host that executes this module.

- python-apt (python 2)
- python3-apt (python 3)
- aptitude (before 2.4)



## Parameters

| Parameter                                                    | Comments                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| **allow_change_held_packages** boolean added in 2.13 of ansible.builtin | Allows changing the version of a package which is on the apt hold list Choices: no ← (default) yes |
| **allow_downgrade** aliases: allow-downgrade, allow_downgrades, allow-downgrades boolean added in 2.12 of ansible.builtin | Corresponds to the `--allow-downgrades` option for *apt*. This option enables the named package and version to replace an already installed higher version of that package. Note that setting *allow_downgrade=true* can make this module behave in a non-idempotent way. (The task could end up with a set of packages that does not match the complete list of specified packages to install). Choices: no ← (default) yes |
| **allow_unauthenticated** aliases: allow-unauthenticated boolean added in 2.1 of ansible.builtin | Ignore if packages cannot be authenticated. This is useful for bootstrapping environments that manage their own apt-key setup. `allow_unauthenticated` is only supported with state: *install*/*present* Choices: no ← (default) yes |
| **autoclean** boolean added in 2.4 of ansible.builtin        | If `yes`, cleans the local repository of retrieved package files that can no longer be downloaded. Choices: no ← (default) yes |
| **autoremove** boolean added in 2.1 of ansible.builtin       | If `yes`, remove unused dependency packages for all module states except *build-dep*. It can also be used as the only option. Previous to version 2.4, autoclean was also an alias for autoremove,  now it is its own separate command. See documentation for further  information. Choices: no ← (default) yes |
| **cache_valid_time** integer                                 | Update the apt cache if it is older than the *cache_valid_time*. This option is set in seconds. As of Ansible 2.4, if explicitly set, this sets *update_cache=yes*. Default: 0 |
| **clean** boolean added in 2.13 of ansible.builtin           | Run the equivalent of `apt-get clean` to clear out the local repository of retrieved package files. It  removes everything but the lock file from /var/cache/apt/archives/ and  /var/cache/apt/archives/partial/. Can be run as part of the package installation (clean runs before install) or as a separate step. Choices: no ← (default) yes |
| **deb** path added in 1.6 of ansible.builtin                 | Path to a .deb package on the remote machine. If :// in the path, ansible will attempt to download deb before installing. (Version added 2.1) Requires the `xz-utils` package to extract the control file of the deb package to install. |
| **default_release** aliases: default-release string          | Corresponds to the `-t` option for *apt* and sets pin priorities |
| **dpkg_options** string                                      | Add dpkg options to apt command. Defaults to ‘-o “Dpkg::Options::=–force-confdef” -o “Dpkg::Options::=–force-confold”’ Options should be supplied as comma separated list Default: “force-confdef,force-confold” |
| **fail_on_autoremove** boolean added in 2.11 of ansible.builtin | Corresponds to the `--no-remove` option for `apt`. If `yes`, it is ensured that no packages will be removed or the task will fail. `fail_on_autoremove` is only supported with state except `absent` Choices: no ← (default) yes |
| **force** boolean                                            | Corresponds to the `--force-yes` to *apt-get* and implies `allow_unauthenticated: yes` and `allow_downgrade: yes` This option will disable checking both the packages’ signatures and  the certificates of the web servers they are downloaded from. This option *is not* the equivalent of passing the `-f` flag to *apt-get* on the command line **This is a destructive operation with the potential to destroy your system, and it should almost never be used.** Please also see `man apt-get` for more information. Choices: no ← (default) yes |
| **force_apt_get** boolean added in 2.4 of ansible.builtin    | Force usage of apt-get instead of aptitude Choices: no ← (default) yes |
| **install_recommends** aliases: install-recommends boolean   | Corresponds to the `--no-install-recommends` option for *apt*. `yes` installs recommended packages.  `no` does not install recommended packages. By default, Ansible will use the same defaults as the operating system. Suggested packages are never  installed. Choices: no yes |
| **lock_timeout** integer added in 2.12 of ansible.builtin    | How many seconds will this action wait to acquire a lock on the apt db. Sometimes there is a transitory lock and this will retry at least until timeout is hit. Default: 60 |
| **name** aliases: package, pkg list / elements=string        | A list of package names, like `foo`, or package specifier with version, like `foo=1.0` or `foo>=1.0`. Name wildcards (fnmatch) like `apt*` and version wildcards like `foo=1.0*` are also supported. |
| **only_upgrade** boolean added in 2.1 of ansible.builtin     | Only upgrade a package if it is already installed. Choices: no ← (default) yes |
| **policy_rc_d** integer added in 2.8 of ansible.builtin      | Force the exit code of /usr/sbin/policy-rc.d. For example, if *policy_rc_d=101* the installed package will not trigger a service start. If /usr/sbin/policy-rc.d already exists, it is backed up and restored after the package installation. If `null`, the /usr/sbin/policy-rc.d isn’t created/changed. |
| **purge** boolean                                            | Will force purging of configuration files if the module state is set to *absent*. Choices: no ← (default) yes |
| **state** string                                             | Indicates the desired package state. `latest` ensures that the latest version is installed. `build-dep` ensures the package build dependencies are installed. `fixed` attempt to correct a system with broken dependencies in place. Choices: absent build-dep latest present ← (default) fixed |
| **update_cache** aliases: update-cache boolean               | Run the equivalent of `apt-get update` before the operation. Can be run as part of the package installation or as a separate step. Default is not to update the cache. Choices: no yes |
| **update_cache_retries** integer added in 2.10 of ansible.builtin | Amount of retries if the cache update fails. Also see *update_cache_retry_max_delay*. Default: 5 |
| **update_cache_retry_max_delay** integer added in 2.10 of ansible.builtin | Use an exponential backoff delay for each retry (see *update_cache_retries*) up to this max delay in seconds. Default: 12 |
| **upgrade** string added in 1.1 of ansible.builtin           | If yes or safe, performs an aptitude safe-upgrade. If full, performs an aptitude full-upgrade. If dist, performs an apt-get dist-upgrade. Note: This does not upgrade a specific package, use state=latest for that. Note: Since 2.4, apt-get is used as a fall-back if aptitude is not present. Choices: dist full no ← (default) safe yes |



## Attributes

| Attribute      | Support          | Description                                                  |
| -------------- | ---------------- | ------------------------------------------------------------ |
| **check_mode** | full             | Can run in check_mode and return changed status prediction without modifying target |
| **diff_mode**  | full             | Will return details on what has changed (or possibly needs changing in check_mode), when in diff mode |
| **platform**   | Platform: debian | Target OS/families that can be operated against              |



## Notes

- Three of the upgrade modes (`full`, `safe` and its alias `yes`) required `aptitude` up to 2.3, since 2.4 `apt-get` is used as a fall-back.
- In most cases, packages installed with apt will start newly  installed services by default. Most distributions have mechanisms to  avoid this. For example when installing Postgresql-9.5 in Debian 9,  creating an excutable shell script (/usr/sbin/policy-rc.d) that throws a return code of 101 will stop Postgresql 9.5 starting up after install.  Remove the file or remove its execute permission afterwards.
- The apt-get commandline supports implicit regex matches here but we do not because it can let typos through easier (If you typo `foo` as `fo` apt-get would install packages that have “fo” in their name with a  warning and a prompt for the user. Since we don’t have warnings and  prompts before installing we disallow this.Use an explicit fnmatch  pattern if you want wildcarding)
- When used with a `loop:` each package will be processed individually, it is much more efficient to pass the list directly to the *name* option.



## Examples

```ini
- name: Install apache httpd  (state=present is optional)
  ansible.builtin.apt:
    name: apache2
    state: present

- name: Update repositories cache and install "foo" package
  ansible.builtin.apt:
    name: foo
    update_cache: yes

- name: Remove "foo" package
  ansible.builtin.apt:
    name: foo
    state: absent

- name: Install the package "foo"
  ansible.builtin.apt:
    name: foo

- name: Install a list of packages
  ansible.builtin.apt:
    pkg:
    - foo
    - foo-tools

- name: Install the version '1.00' of package "foo"
  ansible.builtin.apt:
    name: foo=1.00

- name: Update the repository cache and update package "nginx" to latest version using default release squeeze-backport
  ansible.builtin.apt:
    name: nginx
    state: latest
    default_release: squeeze-backports
    update_cache: yes

- name: Install the version '1.18.0' of package "nginx" and allow potential downgrades
  ansible.builtin.apt:
    name: nginx=1.18.0
    state: present
    allow_downgrade: yes

- name: Install zfsutils-linux with ensuring conflicted packages (e.g. zfs-fuse) will not be removed.
  ansible.builtin.apt:
    name: zfsutils-linux
    state: latest
    fail_on_autoremove: yes

- name: Install latest version of "openjdk-6-jdk" ignoring "install-recommends"
  ansible.builtin.apt:
    name: openjdk-6-jdk
    state: latest
    install_recommends: no

- name: Update all packages to their latest version
  ansible.builtin.apt:
    name: "*"
    state: latest

- name: Upgrade the OS (apt-get dist-upgrade)
  ansible.builtin.apt:
    upgrade: dist

- name: Run the equivalent of "apt-get update" as a separate step
  ansible.builtin.apt:
    update_cache: yes

- name: Only run "update_cache=yes" if the last one is more than 3600 seconds ago
  ansible.builtin.apt:
    update_cache: yes
    cache_valid_time: 3600

- name: Pass options to dpkg on run
  ansible.builtin.apt:
    upgrade: dist
    update_cache: yes
    dpkg_options: 'force-confold,force-confdef'

- name: Install a .deb package
  ansible.builtin.apt:
    deb: /tmp/mypackage.deb

- name: Install the build dependencies for package "foo"
  ansible.builtin.apt:
    pkg: foo
    state: build-dep

- name: Install a .deb package from the internet
  ansible.builtin.apt:
    deb: https://example.com/python-ppq_0.1-1_all.deb

- name: Remove useless packages from the cache
  ansible.builtin.apt:
    autoclean: yes

- name: Remove dependencies that are no longer required
  ansible.builtin.apt:
    autoremove: yes

- name: Run the equivalent of "apt-get clean" as a separate step
  apt:
    clean: yes
```