# Git module 

Deploy software (or files) from git checkouts



## Synopsis

- Manage *git* checkouts of repositories to deploy files or software.

## Requirements

The below requirements are needed on the host that executes this module.

- git>=1.7.1 (the command line tool)



## Parameters

| Parameter                                                    | Comments                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| **accept_hostkey** boolean added in 1.5 of ansible.builtin   | Will ensure or not that “-o StrictHostKeyChecking=no” is present as an ssh option. Be aware that this disables a protection against MITM attacks. Those using OpenSSH >= 7.5 might want to set *ssh_opt* to ‘StrictHostKeyChecking=accept-new’ instead, it does not remove the MITM issue but it does restrict it to the first attempt. Choices: no ← (default) yes |
| **accept_newhostkey** boolean added in 2.12 of ansible.builtin | As of OpenSSH 7.5, “-o  StrictHostKeyChecking=accept-new” can be used which is safer and will  only accepts host keys which are not present or are the same. if `yes`, ensure that “-o StrictHostKeyChecking=accept-new” is present as an ssh option. Choices: no ← (default) yes |
| **archive** path added in 2.4 of ansible.builtin             | Specify archive file path with  extension. If specified, creates an archive file of the specified format containing the tree structure for the source tree. Allowed archive  formats [“zip”, “tar.gz”, “tar”, “tgz”]. This will clone and perform git archive from local directory as not all git servers support git archive. |
| **archive_prefix** string added in 2.10 of ansible.builtin   | Specify a prefix to add to each file path in archive. Requires *archive* to be specified. |
| **bare** boolean added in 1.4 of ansible.builtin             | If `yes`, repository will be created as a bare repo, otherwise it will be a standard repo with a workspace. Choices: no ← (default) yes |
| **clone** boolean added in 1.9 of ansible.builtin            | If `no`, do not clone the repository even if it does not exist locally. Choices: no yes ← (default) |
| **depth** integer added in 1.2 of ansible.builtin            | Create a shallow clone with a history truncated to the specified number or revisions. The minimum possible value is `1`, otherwise ignored. Needs *git>=1.9.1* to work correctly. |
| **dest** path / required                                     | The path of where the repository should be checked out. This is equivalent to `git clone [repo_url] [directory]`. The repository named in *repo* is not appended to this path and the destination directory must be empty. This parameter is required, unless *clone* is set to `no`. |
| **executable** path added in 1.4 of ansible.builtin          | Path to git executable to use. If not supplied, the normal mechanism for resolving binary paths will be used. |
| **force** boolean added in 0.7 of ansible.builtin            | If `yes`, any modified files in the working repository will be discarded.  Prior to 0.7, this was always `yes` and could not be disabled.  Prior to 1.9, the default was `yes`. Choices: no ← (default) yes |
| **gpg_whitelist** list / elements=string added in 2.9 of ansible.builtin | A list of trusted GPG fingerprints to compare to the fingerprint of the GPG-signed commit. Only used when *verify_commit=yes*. Use of this feature requires Git 2.6+ due to its reliance on git’s `--raw` flag to `verify-commit` and `verify-tag`. Default: [] |
| **key_file** path added in 1.5 of ansible.builtin            | Specify an optional private key file path, on the target host, to use for the checkout. This ensures ‘IdentitiesOnly=yes’ is present in ssh_opts. |
| **recursive** boolean added in 1.6 of ansible.builtin        | If `no`, repository will be cloned without the –recursive option, skipping sub-modules. Choices: no yes ← (default) |
| **reference** string added in 1.4 of ansible.builtin         | Reference repository (see “git clone –reference …”).         |
| **refspec** string added in 1.9 of ansible.builtin           | Add an additional refspec to be fetched. If version is set to a *SHA-1* not reachable from any branch or tag, this option may be necessary to specify the ref containing the *SHA-1*. Uses the same syntax as the `git fetch` command. An example value could be “refs/meta/config”. |
| **remote** string                                            | Name of the remote. Default: “origin”                        |
| **repo** aliases: name string / required                     | git, SSH, or HTTP(S) protocol address of the git repository. |
| **separate_git_dir** path added in 2.7 of ansible.builtin    | The path to place the cloned repository. If specified, Git repository can be separated from working tree. |
| **single_branch** boolean added in 2.11 of ansible.builtin   | Clone only the history leading to the tip of the specified revision. Choices: no ← (default) yes |
| **ssh_opts** string added in 1.5 of ansible.builtin          | Options git will pass to ssh when used as protocol, it works via `git`‘s GIT_SSH/GIT_SSH_COMMAND environment variables. For older versions it appends GIT_SSH_OPTS (specific to this module) to the variables above or via a wrapper script. Other options can add to this list, like *key_file* and *accept_hostkey*. An example value could be “-o StrictHostKeyChecking=no” (although this particular option is better set by *accept_hostkey*). The module ensures that ‘BatchMode=yes’ is always present to avoid prompts. |
| **track_submodules** boolean added in 1.8 of ansible.builtin | If `yes`, submodules will track the latest commit on their master branch (or other branch specified in .gitmodules).  If `no`, submodules will be kept at the revision specified by the main project.  This is equivalent to specifying the –remote flag to git submodule  update. Choices: no ← (default) yes |
| **umask** raw added in 2.2 of ansible.builtin                | The umask to set before doing any checkouts, or any other repository maintenance. |
| **update** boolean added in 1.2 of ansible.builtin           | If `no`, do not retrieve new revisions from the origin repository. Operations like archive will work on the existing (old) repository  and might not respond to changes to the options version or remote. Choices: no yes ← (default) |
| **verify_commit** boolean added in 2.0 of ansible.builtin    | If `yes`, when cloning or checking out a *version* verify the signature of a GPG signed commit. This requires git  version>=2.1.0 to be installed. The commit MUST be signed and the  public key MUST be present in the GPG keyring. Choices: no ← (default) yes |
| **version** string                                           | What version of the repository to check out. This can be the literal string `HEAD`, a branch name, a tag name. It can also be a *SHA-1* hash, in which case *refspec* needs to be specified if the given revision is not already available. Default: “HEAD” |

## Attributes

| Attribute      | Support         | Description                                                  |
| -------------- | --------------- | ------------------------------------------------------------ |
| **check_mode** | full            | Can run in check_mode and return changed status prediction without modifying target |
| **diff_mode**  | full            | Will return details on what has changed (or possibly needs changing in check_mode), when in diff mode |
| **platform**   | Platform: posix | Target OS/families that can be operated against              |

## Notes

If the task seems to be hanging, first verify remote host is in `known_hosts`. SSH will prompt user to authorize the first contact with a remote host.  To avoid this prompt, one solution is to use the option  accept_hostkey. Another solution is to add the remote host public key in `/etc/ssh/ssh_known_hosts` before calling the git module, with the following command: ssh-keyscan -H remote_host.com >> /etc/ssh/ssh_known_hosts.

## Examples

```ini
- name: Git checkout
  ansible.builtin.git:
    repo: 'https://foosball.example.org/path/to/repo.git'
    dest: /srv/checkout
    version: release-0.22

- name: Read-write git checkout from github
  ansible.builtin.git:
    repo: git@github.com:mylogin/hello.git
    dest: /home/mylogin/hello

- name: Just ensuring the repo checkout exists
  ansible.builtin.git:
    repo: 'https://foosball.example.org/path/to/repo.git'
    dest: /srv/checkout
    update: no

- name: Just get information about the repository whether or not it has already been cloned locally
  ansible.builtin.git:
    repo: 'https://foosball.example.org/path/to/repo.git'
    dest: /srv/checkout
    clone: no
    update: no

- name: Checkout a github repo and use refspec to fetch all pull requests
  ansible.builtin.git:
    repo: https://github.com/ansible/ansible-examples.git
    dest: /src/ansible-examples
    refspec: '+refs/pull/*:refs/heads/*'

- name: Create git archive from repo
  ansible.builtin.git:
    repo: https://github.com/ansible/ansible-examples.git
    dest: /src/ansible-examples
    archive: /tmp/ansible-examples.zip

- name: Clone a repo with separate git directory
  ansible.builtin.git:
    repo: https://github.com/ansible/ansible-examples.git
    dest: /src/ansible-examples
    separate_git_dir: /src/ansible-examples.git

- name: Example clone of a single branch
  ansible.builtin.git:
    repo: https://github.com/ansible/ansible-examples.git
    dest: /src/ansible-examples
    single_branch: yes
    version: master

- name: Avoid hanging when http(s) password is missing
  ansible.builtin.git:
    repo: https://github.com/ansible/could-be-a-private-repo
    dest: /src/from-private-repo
  environment:
    GIT_TERMINAL_PROMPT: 0 # reports "terminal prompts disabled" on missing password
    # or GIT_ASKPASS: /bin/true # for git before version 2.3.0, reports "Authentication failed" on missing password
```