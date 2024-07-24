# Ansible-Vault

  **Encryption/decryption utility for Ansible data files**

- **Keeping sensitive data such as passwords or keys in encrypted files, rather than as plaintext in your playbooks or roles.**
- **any structured data file can be encrypted like tasks, handlers, vars, bin,...**
- **an encrypted file as the 'src' argument of copy, template, unarchive and script modules automatically can be decrypted on the target.**



## Synopsis

```bash
usage: $ ansible-vault [-h] [--version] [-v]
                  {create,decrypt,edit,view,encrypt,encrypt_string,rekey}
                  ...
```



## Description

can encrypt any structured data file used by Ansible. This can include *group_vars/* or *host_vars/* inventory variables, variables loaded by *include_vars* or *vars_files*, or variable files passed on the ansible-playbook command line with *-e @file.yml* or *-e @file.json*. Role variables and defaults are also included!

Because Ansible tasks, handlers, and other objects are data, these can also be encrypted with vault. If you’d like to not expose what variables you are using, you can keep an individual task file entirely encrypted.

## Common Options

- --version

  show program’s version number, config file location, configured  module search path, module location, executable location and exit

- -h, --help

  show this help message and exit

- -v, --verbose

  Causes Ansible to print more debug messages. Adding multiple -v  will increase the verbosity, the builtin plugins currently evaluate up  to -vvvvvv. A reasonable level to start is -vvv, connection debugging  might require -vvvv.

## Actions

### create

create and open a file in an editor that will be encrypted with the provided vault secret when closed

- --ask-vault-password, --ask-vault-pass[](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#cmdoption-ansible-vault-create-ask-vault-password)

  ask for vault password

- --encrypt-vault-id <ENCRYPT_VAULT_ID>[](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#cmdoption-ansible-vault-create-encrypt-vault-id)

  the vault id used to encrypt (required if more than one vault-id is provided)

- --vault-id[](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#cmdoption-ansible-vault-create-vault-id)

  the vault identity to use

- --vault-password-file, --vault-pass-file[](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#cmdoption-ansible-vault-create-vault-password-file)

  vault password file



### decrypt

decrypt the supplied file using the provided vault secret

- --ask-vault-password, --ask-vault-pass[](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#cmdoption-ansible-vault-decrypt-ask-vault-password)

  ask for vault password

- --output <OUTPUT_FILE>[](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#cmdoption-ansible-vault-decrypt-output)

  output file name for encrypt or decrypt; use - for stdout

- --vault-id[](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#cmdoption-ansible-vault-decrypt-vault-id)

  the vault identity to use

- --vault-password-file, --vault-pass-file[](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#cmdoption-ansible-vault-decrypt-vault-password-file)

  vault password file



### edit

open and decrypt an existing vaulted file in an editor, that will be encrypted again when closed

- --ask-vault-password, --ask-vault-pass[](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#cmdoption-ansible-vault-edit-ask-vault-password)

  ask for vault password

- --encrypt-vault-id <ENCRYPT_VAULT_ID>[](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#cmdoption-ansible-vault-edit-encrypt-vault-id)

  the vault id used to encrypt (required if more than one vault-id is provided)

- --vault-id[](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#cmdoption-ansible-vault-edit-vault-id)

  the vault identity to use

- --vault-password-file, --vault-pass-file[](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#cmdoption-ansible-vault-edit-vault-password-file)

  vault password file



### view

open, decrypt and view an existing vaulted file using a pager using the supplied vault secret

- --ask-vault-password, --ask-vault-pass[](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#cmdoption-ansible-vault-view-ask-vault-password)

  ask for vault password

- --vault-id[](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#cmdoption-ansible-vault-view-vault-id)

  the vault identity to use

- --vault-password-file, --vault-pass-file[](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#cmdoption-ansible-vault-view-vault-password-file)

  vault password file



### encrypt

encrypt the supplied file using the provided vault secret

- --ask-vault-password, --ask-vault-pass[](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#cmdoption-ansible-vault-encrypt-ask-vault-password)

  ask for vault password

- --encrypt-vault-id <ENCRYPT_VAULT_ID>[](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#cmdoption-ansible-vault-encrypt-encrypt-vault-id)

  the vault id used to encrypt (required if more than one vault-id is provided)

- --output <OUTPUT_FILE>[](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#cmdoption-ansible-vault-encrypt-output)

  output file name for encrypt or decrypt; use - for stdout

- --vault-id[](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#cmdoption-ansible-vault-encrypt-vault-id)

  the vault identity to use

- --vault-password-file, --vault-pass-file[](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#cmdoption-ansible-vault-encrypt-vault-password-file)

  vault password file



### encrypt_string

encrypt the supplied string using the provided vault secret

- --ask-vault-password, --ask-vault-pass[](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#cmdoption-ansible-vault-encrypt_string-ask-vault-password)

  ask for vault password

- --encrypt-vault-id <ENCRYPT_VAULT_ID>[](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#cmdoption-ansible-vault-encrypt_string-encrypt-vault-id)

  the vault id used to encrypt (required if more than one vault-id is provided)

- --output <OUTPUT_FILE>[](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#cmdoption-ansible-vault-encrypt_string-output)

  output file name for encrypt or decrypt; use - for stdout

- --show-input[](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#cmdoption-ansible-vault-encrypt_string-show-input)

  Do not hide input when prompted for the string to encrypt

- --stdin-name <ENCRYPT_STRING_STDIN_NAME>[](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#cmdoption-ansible-vault-encrypt_string-stdin-name)

  Specify the variable name for stdin

- --vault-id[](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#cmdoption-ansible-vault-encrypt_string-vault-id)

  the vault identity to use

- --vault-password-file, --vault-pass-file[](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#cmdoption-ansible-vault-encrypt_string-vault-password-file)

  vault password file

- -n, --name[](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#cmdoption-ansible-vault-encrypt_string-n)

  Specify the variable name

- -p, --prompt[](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html#cmdoption-ansible-vault-encrypt_string-p)

  Prompt for the string to encrypt



### rekey

re-encrypt a vaulted file with a new secret, the previous secret is required

- --ask-vault-password, --ask-vault-pass

  ask for vault password

- --encrypt-vault-id <ENCRYPT_VAULT_ID>

  the vault id used to encrypt (required if more than one vault-id is provided)

- --new-vault-id <NEW_VAULT_ID>

  the new vault identity to use for rekey

- --new-vault-password-file <NEW_VAULT_PASSWORD_FILE>

  new vault password file for rekey

- --vault-id

  the vault identity to use

- --vault-password-file, --vault-pass-file

  vault password file

## Environment

The following environment variables may be specified.

[`ANSIBLE_CONFIG`](https://docs.ansible.com/ansible/latest/reference_appendices/config.html#envvar-ANSIBLE_CONFIG) – Override the default ansible config file

Many more are available for most options in ansible.cfg

## Files

`/etc/ansible/ansible.cfg` – Config file, used if present

`~/.ansible.cfg` – User config file, overrides the default config if present



## Example

Create a new encrypted data file 

```bash
$ Ansible-vault create test.yml
```

Edit a encrypted file in place:

```bash
$ Ansible-vault edit  test.yml
```

Change password on a vault-encrypted file: 

```bash
$ Ansible-vault rekey test.yml
```

Encrypt an existing file

```bash
$ Ansible-vault encrypt test.yml
```

Decrypt an encrypted file permanently

```bash
$ Ansible-vault decrypt test.yml
```

View an encrypted file without editing it

```bash
$ Ansible-vault view test.yml
```

Prompt for a vault password

```bash
$ Ansible-playbook --ask-vault-pass myproject.yml
```

Store a vault password in a text file

```bash
$ Ansible-playbook --vault-password-file pass-file myproject.yml
```



## Single Encrypted Variable

Replace the password in the YML file with the generated encrypted string

```bash
$ ansible-vault encrypt_string test_string --ask-vault-pass
> 
New Vault password: 
Confirm New Vault password: 
!vault |
          $ANSIBLE_VAULT;1.1;AES256
          30323461653935336166633932376165313533363765633535623131616263383435386361646162
          6434376632643962613031363737646364353765393965330a393166316135366131303734386132
          65353866633464313036396364613733333664623466393237353430626363336635643964643362
          3266643664353462390a373633383361323339666132393165303731666131376266643934623438
          3236
Encryption successful
```

 ```yaml
 - name: Set Encrypted string as variable
   set_fact: 
     mypass: !vault |
           $ANSIBLE_VAULT;1.1;AES256
           30323461653935336166633932376165313533363765633535623131616263383435386361646162
           6434376632643962613031363737646364353765393965330a393166316135366131303734386132
           65353866633464313036396364613733333664623466393237353430626363336635643964643362
           3266643664353462390a373633383361323339666132393165303731666131376266643934623438
           3236
    no_log: true
    tags: vault1
 ```

```bash
$ ansible-playbook -i inventory/hosts project.yml  --tags vault1 --ask-vault-pass
```



## Vault IDs

- **Help in encrypting different file with different passwords to be referenced inside a playbook.**

1-Create multiple password files:

```bash
$ echo "123" > ~/.vault-pass.common
$ echo "456" > ~/.vault-pass.staging
$ echo "789" > ~/.vault-pass.production
```

2- Add `vault_identity_list` to `ansible.cfg`:

```bash
$ cat /etc/ansible/ansible.cfg
[default]
vault_identity_list = common@~/.vault-pass.common, staging@~/.vault-pass.staging, production@~/.vault-pass.production
```

3-Encrypt files via vault-id:

```bash
$ ansible-vault encrypt --encrypt-vault-id common test.yml
$ ansible-vault encrypt --encrypt-vault-id stagingtest.yml
$ ansible-vault encrypt --encrypt-vault-id production test.yml
```

4- To run the playbook 

```bash
$ ansible-playbook -i inventory/hosts myproject.yml
```

> Note: if the first password wont open the vault, it will move on to the next one, until one of them works.



- **If you want to be prompted for password to decrypt the vault string/file**

1- Comment out vault_identity_list key in `ansible.cfg`

2- Execute the playbook with --vault-id id@prompt

```bash
$ ansible-palybook -i inventory/host --vault-id common@prompt --vault-id staging@prompt .. myproject.yml
```

