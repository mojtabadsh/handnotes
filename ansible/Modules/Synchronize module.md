# Synchronize module 

A wrapper around rsync to make common tasks in your playbooks quick and easy



## Synopsis

- `synchronize` is a wrapper around rsync to make common tasks in your playbooks quick and easy.
- It is run and originates on the local host where Ansible is being run.
- Of course, you could just use the `command` action to call rsync yourself, but you also have to add a fair number of boilerplate options and host facts.
- This module is not intended to provide access to the full power  of rsync, but does make the most common invocations easier to implement. You still may need to call rsync directly via `command` or `shell` depending on your use case.



> This module has a corresponding [action plugin](https://docs.ansible.com/ansible/latest/plugins/action.html#action-plugins).



## Parameters

| Parameter                                                 | Comments                                                     |
| --------------------------------------------------------- | ------------------------------------------------------------ |
| **archive** boolean                                       | Mirrors the rsync archive flag, enables recursive, links, perms, times, owner, group flags and -D. Choices: no yes ← (default) |
| **checksum** boolean                                      | Skip based on checksum, rather  than mod-time & size; Note that that “archive” option is still  enabled by default - the “checksum” option will not disable it. Choices: no ← (default) yes |
| **compress** boolean                                      | Compress file data during the transfer. In most cases, leave this enabled unless it causes problems. Choices: no yes ← (default) |
| **copy_links** boolean                                    | Copy symlinks as the item that they point to (the referent) is copied, rather than the symlink. Choices: no ← (default) yes |
| **delay_updates** boolean added in 1.3.0 of ansible.posix | This option puts the temporary  file from each updated file into a holding directory until the end of  the transfer, at which time all the files are renamed into place in  rapid succession. Choices: no yes ← (default) |
| **delete** boolean                                        | Delete files in *dest* that do not exist (after transfer, not before) in the *src* path. This option requires *recursive=yes*. This option ignores excluded files and behaves like the rsync opt `--delete-after`. Choices: no ← (default) yes |
| **dest** string / required                                | Path on the destination host that will be synchronized from the source. The path can be absolute or relative. |
| **dest_port** integer                                     | Port number for ssh on the destination host. Prior to Ansible 2.0, the ansible_ssh_port inventory var took precedence over this value. This parameter defaults to the value of `ansible_port`, the `remote_port` config setting or the value from ssh client configuration if none of the former have been set. |
| **dirs** boolean                                          | Transfer directories without recursing. Choices: no ← (default) yes |
| **existing_only** boolean                                 | Skip creating new files on receiver. Choices: no ← (default) yes |
| **group** boolean                                         | Preserve group. This parameter defaults to the value of the archive option. Choices: no yes |
| **link_dest** list / elements=string                      | Add a destination to hard link against during the rsync.     |
| **links** boolean                                         | Copy symlinks as symlinks. This parameter defaults to the value of the archive option. Choices: no yes |
| **mode** string                                           | Specify the direction of the synchronization. In push mode the localhost or delegate is the source. In pull mode the remote host in context is the source. Choices: pull push ← (default) |
| **owner** boolean                                         | Preserve owner (super user only). This parameter defaults to the value of the archive option. Choices: no yes |
| **partial** boolean                                       | Tells rsync to keep the partial file which should make a subsequent transfer of the rest of the file much faster. Choices: no ← (default) yes |
| **perms** boolean                                         | Preserve permissions. This parameter defaults to the value of the archive option. Choices: no yes |
| **private_key** path                                      | Specify the private key to use for SSH-based rsync connections (e.g. `~/.ssh/id_rsa`). |
| **recursive** boolean                                     | Recurse into directories. This parameter defaults to the value of the archive option. Choices: no yes |
| **rsync_opts** list / elements=string                     | Specify additional rsync options by passing in an array. Note that an empty string in `rsync_opts` will end up transfer the current working directory. |
| **rsync_path** string                                     | Specify the rsync command to run on the remote host. See `--rsync-path` on the rsync man page. To specify the rsync command to run on the local host, you need to set this your task var `ansible_rsync_path`. |
| **rsync_timeout** integer                                 | Specify a `--timeout` for the rsync command in seconds. Default: 0 |
| **set_remote_user** boolean                               | Put user@ for the remote paths. If you have a custom ssh config to define the remote user for a host  that does not match the inventory user, you should set this parameter to `no`. Choices: no yes ← (default) |
| **src** string / required                                 | Path on the source host that will be synchronized to the destination. The path can be absolute or relative. |
| **ssh_connection_multiplexing** boolean                   | SSH connection multiplexing for  rsync is disabled by default to prevent misconfigured ControlSockets  from resulting in failed SSH connections. This is accomplished by  setting the SSH `ControlSocket` to `none`. Set this option to `yes` to allow multiplexing and reduce SSH connection overhead. Note that simply setting this option to `yes` is not enough; You must also configure SSH connection multiplexing in your SSH client config by setting values for `ControlMaster`, `ControlPersist` and `ControlPath`. Choices: no ← (default) yes |
| **times** boolean                                         | Preserve modification times. This parameter defaults to the value of the archive option. Choices: no yes |
| **use_ssh_args** boolean                                  | In Ansible 2.10 and lower, it uses the ssh_args specified in `ansible.cfg`. In Ansible 2.11 and onwards, when set to `true`, it uses all SSH connection configurations like `ansible_ssh_args`, `ansible_ssh_common_args`, and `ansible_ssh_extra_args`. Choices: no ← (default) yes |
| **verify_host** boolean                                   | Verify destination host key. Choices: no ← (default) yes     |



## **Note**

- rsync must be installed on both the local and remote host.
- For the `synchronize` module, the “local host” is the host the synchronize task originates on, and the “destination host” is the host synchronize is connecting to.
- The “local host” can be changed to a different host by using delegate_to.  This enables copying between two remote hosts or entirely on one remote machine.
- The user and permissions for the synchronize src are those of the user running the Ansible task on the local host (or the  remote_user for a delegate_to host when delegate_to is used).
- The user and permissions for the synchronize dest are those of the remote_user on the destination host or the become_user if become=yes is active.
- In Ansible 2.0 a bug in the synchronize module made become occur on the “local host”.  This was fixed in Ansible 2.0.1.
- Currently, synchronize is limited to elevating permissions via  passwordless sudo.  This is because rsync itself is connecting to the  remote machine and rsync doesn’t give us a way to pass sudo credentials  in.
- Currently there are only a few connection types which support  synchronize (ssh, paramiko, local, and docker) because a sync strategy  has been determined for those connection types.  Note that the  connection for these must not need a password as rsync itself is making  the connection and rsync does not provide us a way to pass a password to the connection.
- Expect that dest=~/x will be ~<remote_user>/x even if using sudo.
- Inspect the verbose output to validate the destination user/host/path are what was expected.
- To exclude files and directories from being synchronized, you may add `.rsync-filter` files to the source directory.
- rsync daemon must be up and running with correct permission when using rsync protocol in source or destination path.
- The `synchronize` module enables –delay-updates by default to avoid leaving a destination in a broken in-between state  if the underlying rsync process encounters an error. Those synchronizing large numbers of files that are willing to trade safety for performance should disable this option.
- link_destination is subject to the same limitations as the  underlying rsync daemon. Hard links are only preserved if the relative  subtrees of the source and destination are the same. Attempts to  hardlink into a directory that is a subdirectory of the source will be  prevented.

## 

## Examples

```ini
- name: Synchronization of src on the control machine to dest on the remote hosts
  ansible.posix.synchronize:
    src: some/relative/path
    dest: /some/absolute/path

- name: Synchronization using rsync protocol (push)
  ansible.posix.synchronize:
    src: some/relative/path/
    dest: rsync://somehost.com/path/

- name: Synchronization using rsync protocol (pull)
  ansible.posix.synchronize:
    mode: pull
    src: rsync://somehost.com/path/
    dest: /some/absolute/path/

- name:  Synchronization using rsync protocol on delegate host (push)
  ansible.posix.synchronize:
    src: /some/absolute/path/
    dest: rsync://somehost.com/path/
  delegate_to: delegate.host

- name: Synchronization using rsync protocol on delegate host (pull)
  ansible.posix.synchronize:
    mode: pull
    src: rsync://somehost.com/path/
    dest: /some/absolute/path/
  delegate_to: delegate.host

- name: Synchronization without any --archive options enabled
  ansible.posix.synchronize:
    src: some/relative/path
    dest: /some/absolute/path
    archive: no

- name: Synchronization with --archive options enabled except for --recursive
  ansible.posix.synchronize:
    src: some/relative/path
    dest: /some/absolute/path
    recursive: no

- name: Synchronization with --archive options enabled except for --times, with --checksum option enabled
  ansible.posix.synchronize:
    src: some/relative/path
    dest: /some/absolute/path
    checksum: yes
    times: no

- name: Synchronization without --archive options enabled except use --links
  ansible.posix.synchronize:
    src: some/relative/path
    dest: /some/absolute/path
    archive: no
    links: yes

- name: Synchronization of two paths both on the control machine
  ansible.posix.synchronize:
    src: some/relative/path
    dest: /some/absolute/path
  delegate_to: localhost

- name: Synchronization of src on the inventory host to the dest on the localhost in pull mode
  ansible.posix.synchronize:
    mode: pull
    src: some/relative/path
    dest: /some/absolute/path

- name: Synchronization of src on delegate host to dest on the current inventory host.
  ansible.posix.synchronize:
    src: /first/absolute/path
    dest: /second/absolute/path
  delegate_to: delegate.host

- name: Synchronize two directories on one remote host.
  ansible.posix.synchronize:
    src: /first/absolute/path
    dest: /second/absolute/path
  delegate_to: "{{ inventory_hostname }}"

- name: Synchronize and delete files in dest on the remote host that are not found in src of localhost.
  ansible.posix.synchronize:
    src: some/relative/path
    dest: /some/absolute/path
    delete: yes
    recursive: yes

# This specific command is granted su privileges on the destination
- name: Synchronize using an alternate rsync command
  ansible.posix.synchronize:
    src: some/relative/path
    dest: /some/absolute/path
    rsync_path: su -c rsync

# Example .rsync-filter file in the source directory
# - var       # exclude any path whose last part is 'var'
# - /var      # exclude any path starting with 'var' starting at the source directory
# + /var/conf # include /var/conf even though it was previously excluded

- name: Synchronize passing in extra rsync options
  ansible.posix.synchronize:
    src: /tmp/helloworld
    dest: /var/www/helloworld
    rsync_opts:
      - "--no-motd"
      - "--exclude=.git"

# Hardlink files if they didn't change
- name: Use hardlinks when synchronizing filesystems
  ansible.posix.synchronize:
    src: /tmp/path_a/foo.txt
    dest: /tmp/path_b/foo.txt
    link_dest: /tmp/path_a/

# Specify the rsync binary to use on remote host and on local host
- hosts: groupofhosts
  vars:
    ansible_rsync_path: /usr/gnu/bin/rsync

  tasks:
    - name: copy /tmp/localpath/ to remote location /tmp/remotepath
      ansible.posix.synchronize:
        src: /tmp/localpath/
        dest: /tmp/remotepath
        rsync_path: /usr/gnu/bin/rsync
```