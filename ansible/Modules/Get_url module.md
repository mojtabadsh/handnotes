# Get_url module 

Downloads files from HTTP, HTTPS, or FTP to node

## Synopsis

- Downloads files from HTTP, HTTPS, or FTP to the remote server. The remote server *must* have direct access to the remote resource.
- By default, if an environment variable `<protocol>_proxy` is set on the target host, requests will be sent through that proxy.  This behaviour can be overridden by setting a variable for this task  (see [setting the environment](https://docs.ansible.com/ansible/latest/user_guide/playbooks_environment.html#playbooks-environment)), or by using the use_proxy option.
- HTTP redirects can redirect from HTTP to HTTPS so you should be sure that your proxy environment for both protocols is correct.
- From Ansible 2.4 when run with `--check`, it will do a HEAD request to validate the URL but will not download the entire file or verify it against hashes and will report incorrect  changed status.
- For Windows targets, use the [ansible.windows.win_get_url](https://docs.ansible.com/ansible/latest/collections/ansible/windows/win_get_url_module.html#ansible-collections-ansible-windows-win-get-url-module) module instead.



## Parameters

| Parameter                                                    | Comments                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| **attributes** aliases: attr string added in 2.3 of ansible.builtin | The attributes the resulting filesystem object should have. To get supported flags look at the man page for *chattr* on the target system. This string should contain the attributes in the same order as the one displayed by *lsattr*. The `=` operator is assumed as default, otherwise `+` or `-` operators need to be included in the string. |
| **backup** boolean added in 2.1 of ansible.builtin           | Create a backup file including  the timestamp information so you can get the original file back if you  somehow clobbered it incorrectly. Choices: no ← (default) yes |
| **checksum** string added in 2.0 of ansible.builtin          | If a checksum is passed to this  parameter, the digest of the destination file will be calculated after  it is downloaded to ensure its integrity and verify that the transfer  completed successfully. Format: <algorithm>:<checksum\|url>,  e.g. checksum=”sha256:D98291AC[…]B6DC7B97”,  checksum=”sha256:http://example.com/path/sha256sum.txt” If you worry about portability, only the sha1 algorithm is available on all platforms and python versions. The third party hashlib library can be installed for access to additional algorithms. Additionally, if a checksum is passed to this parameter, and the file exist under the `dest` location, the *destination_checksum* would be calculated, and if checksum equals *destination_checksum*, the file download would be skipped (unless `force` is true). If the checksum does not equal *destination_checksum*, the destination file is deleted. Default: “” |
| **client_cert** path added in 2.4 of ansible.builtin         | PEM formatted certificate chain file to be used for SSL client authentication. This file can also include the key as well, and if the key is included, `client_key` is not required. |
| **client_key** path added in 2.4 of ansible.builtin          | PEM formatted file that contains your private key to be used for SSL client authentication. If `client_cert` contains both the certificate and key, this option is not required. |
| **dest** path / required                                     | Absolute path of where to download the file to. If `dest` is a directory, either the server provided filename or, if none  provided, the base name of the URL on the remote server will be used. If a directory, `force` has no effect. If `dest` is a directory, the file will always be downloaded (regardless of the `force` and `checksum` option), but replaced only if the contents changed. |
| **force** boolean added in 0.7 of ansible.builtin            | If `yes` and `dest` is not a directory, will download the file every time and replace the file if the contents change. If `no`, the file will only be downloaded if the destination does not exist. Generally should be `yes` only for small local files. Prior to 0.6, this module behaved as if `yes` was the default. Choices: no ← (default) yes |
| **force_basic_auth** boolean added in 2.0 of ansible.builtin | Force the sending of the Basic authentication header upon initial request. httplib2, the library used by the uri module only sends  authentication information when a webservice responds to an initial  request with a 401 status. Since some basic auth services do not  properly send a 401, logins will fail. Choices: no ← (default) yes |
| **group** string                                             | Name of the group that should own the filesystem object, as would be fed to *chown*. |
| **headers** dictionary added in 2.0 of ansible.builtin       | Add custom HTTP headers to a request in hash/dict format. The hash/dict format was added in Ansible 2.6. Previous versions used a `"key:value,key:value"` string format. The `"key:value,key:value"` string format is deprecated and has been removed in version 2.10. |
| **http_agent** string                                        | Header to identify as, generally appears in web server logs. Default: “ansible-httpget” |
| **mode** raw                                                 | The permissions the resulting filesystem object should have. For those used to */usr/bin/chmod* remember that modes are  actually octal numbers. You must either add a leading zero so that  Ansible’s YAML parser knows it is an octal number (like `0644` or `01777`) or quote it (like `'644'` or `'1777'`) so Ansible receives a string and can do its own conversion from string into number. Giving Ansible a number without following one of these rules will end up with a decimal number which will have unexpected results. As of Ansible 1.8, the mode may be specified as a symbolic mode (for example, `u+rwx` or `u=rw,g=r,o=r`). If `mode` is not specified and the destination filesystem object **does not** exist, the default `umask` on the system will be used when setting the mode for the newly created filesystem object. If `mode` is not specified and the destination filesystem object **does** exist, the mode of the existing filesystem object will be used. Specifying `mode` is the best way to ensure filesystem objects are created with the correct permissions. See CVE-2020-1736 for further details. |
| **owner** string                                             | Name of the user that should own the filesystem object, as would be fed to *chown*. |
| **selevel** string                                           | The level part of the SELinux filesystem object context. This is the MLS/MCS attribute, sometimes known as the `range`. When set to `_default`, it will use the `level` portion of the policy if available. |
| **serole** string                                            | The role part of the SELinux filesystem object context. When set to `_default`, it will use the `role` portion of the policy if available. |
| **setype** string                                            | The type part of the SELinux filesystem object context. When set to `_default`, it will use the `type` portion of the policy if available. |
| **seuser** string                                            | The user part of the SELinux filesystem object context. By default it uses the `system` policy, where applicable. When set to `_default`, it will use the `user` portion of the policy if available. |
| **sha256sum** string added in 1.3 of ansible.builtin         | If a SHA-256 checksum is passed  to this parameter, the digest of the destination file will be calculated after it is downloaded to ensure its integrity and verify that the  transfer completed successfully. This option is deprecated and will be  removed in version 2.14. Use option `checksum` instead. Default: “” |
| **timeout** integer added in 1.8 of ansible.builtin          | Timeout in seconds for URL request. Default: 10              |
| **tmp_dest** path added in 2.1 of ansible.builtin            | Absolute path of where temporary file is downloaded to. When run on Ansible 2.5 or greater, path defaults to ansible’s remote_tmp setting When run on Ansible prior to 2.5, it defaults to `TMPDIR`, `TEMP` or `TMP` env variables or a platform specific value. https://docs.python.org/3/library/tempfile.html#tempfile.tempdir |
| **unredirected_headers** list / elements=string added in 2.12 of ansible.builtin | A list of header names that will not be sent on subsequent redirected requests. This list is case  insensitive. By default all headers will be redirected. In some cases it may be beneficial to list headers such as `Authorization` here to avoid potential credential exposure. Default: [] |
| **unsafe_writes** boolean added in 2.2 of ansible.builtin    | Influence when to use atomic operation to prevent data corruption or inconsistent reads from the target filesystem object. By default this module uses atomic operations to prevent data  corruption or inconsistent reads from the target filesystem objects, but sometimes systems are configured or just broken in ways that prevent  this. One example is docker mounted filesystem objects, which cannot be  updated atomically from inside the container and can only be written in  an unsafe manner. This option allows Ansible to fall back to unsafe methods of updating filesystem objects when atomic operations fail (however, it doesn’t  force Ansible to perform unsafe writes). IMPORTANT! Unsafe writes are subject to race conditions and can lead to data corruption. Choices: no ← (default) yes |
| **url** string / required                                    | HTTP, HTTPS, or FTP URL in the form (http\|https\|ftp)://[user[:pass]]@host.domain[:port]/path |
| **url_password** aliases: password string added in 1.6 of ansible.builtin | The password for use in HTTP basic authentication. If the `url_username` parameter is not specified, the `url_password` parameter will not be used. Since version 2.8 you can also use the ‘password’ alias for this option. |
| **url_username** aliases: username string added in 1.6 of ansible.builtin | The username for use in HTTP basic authentication. This parameter can be used without `url_password` for sites that allow empty passwords. Since version 2.8 you can also use the `username` alias for this option. |
| **use_gssapi** boolean added in 2.11 of ansible.builtin      | Use GSSAPI to perform the authentication, typically this is for Kerberos or Kerberos through Negotiate authentication. Requires the Python library [gssapi](https://github.com/pythongssapi/python-gssapi) to be installed. Credentials for GSSAPI can be specified with *url_username*/*url_password* or with the GSSAPI env var `KRB5CCNAME` that specified a custom Kerberos credential cache. NTLM authentication is *not* supported even if the GSSAPI mech for NTLM has been installed. Choices: no ← (default) yes |
| **use_proxy** boolean                                        | if `no`, it will not use a proxy, even if one is defined in an environment variable on the target hosts. Choices: no yes ← (default) |
| **validate_certs** boolean                                   | If `no`, SSL certificates will not be validated. This should only be used on personally controlled sites using self-signed certificates. Choices: no yes ← (default) |



## Note

- For Windows targets, use the [ansible.windows.win_get_url](https://docs.ansible.com/ansible/latest/collections/ansible/windows/win_get_url_module.html#ansible-collections-ansible-windows-win-get-url-module) module instead.

- ficial documentation on the **ansible.windows.win_get_url** module.



## Examples

```ini
- name: Download foo.conf
  ansible.builtin.get_url:
    url: http://example.com/path/file.conf
    dest: /etc/foo.conf
    mode: '0440'

- name: Download file and force basic auth
  ansible.builtin.get_url:
    url: http://example.com/path/file.conf
    dest: /etc/foo.conf
    force_basic_auth: yes

- name: Download file with custom HTTP headers
  ansible.builtin.get_url:
    url: http://example.com/path/file.conf
    dest: /etc/foo.conf
    headers:
      key1: one
      key2: two

- name: Download file with check (sha256)
  ansible.builtin.get_url:
    url: http://example.com/path/file.conf
    dest: /etc/foo.conf
    checksum: sha256:b5bb9d8014a0f9b1d61e21e796d78dccdf1352f23cd32812f4850b878ae4944c

- name: Download file with check (md5)
  ansible.builtin.get_url:
    url: http://example.com/path/file.conf
    dest: /etc/foo.conf
    checksum: md5:66dffb5228a211e61d6d7ef4a86f5758

- name: Download file with checksum url (sha256)
  ansible.builtin.get_url:
    url: http://example.com/path/file.conf
    dest: /etc/foo.conf
    checksum: sha256:http://example.com/path/sha256sum.txt

- name: Download file from a file path
  ansible.builtin.get_url:
    url: file:///tmp/afile.txt
    dest: /tmp/afilecopy.txt

- name: < Fetch file that requires authentication.
        username/password only available since 2.8, in older versions you need to use url_username/url_password
  ansible.builtin.get_url:
    url: http://example.com/path/file.conf
    dest: /etc/foo.conf
    username: bar
    password: '{{ mysecret }}'
```