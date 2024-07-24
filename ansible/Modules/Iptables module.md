# Iptables module 

Modify iptables rules

## Synopsis

- `iptables` is used to set up, maintain, and inspect the tables of IP packet filter rules in the Linux kernel.
- This module does not handle the saving and/or loading of rules,  but rather only manipulates the current rules that are present in  memory. This is the same as the behaviour of the `iptables` and `ip6tables` command which this module uses internally.



## Parameters

| Parameter                                                    | Comments                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| **action** string added in 2.2 of ansible.builtin            | Whether the rule should be appended at the bottom or inserted at the top. If the rule already exists the chain will not be modified. Choices: append ← (default) insert |
| **chain** string                                             | Specify the iptables chain to modify. This could be a user-defined chain or one of the standard iptables chains, like `INPUT`, `FORWARD`, `OUTPUT`, `PREROUTING`, `POSTROUTING`, `SECMARK` or `CONNSECMARK`. |
| **chain_management** boolean added in 2.13 of ansible.builtin | If `true` and `state` is `present`, the chain will be created if needed. If `true` and `state` is `absent`, the chain will be deleted if the only other parameter passed are `chain` and optionally `table`. Choices: no ← (default) yes |
| **comment** string                                           | This specifies a comment that will be added to the rule.     |
| **ctstate** list / elements=string                           | A list of the connection states to match in the conntrack module. Possible values are `INVALID`, `NEW`, `ESTABLISHED`, `RELATED`, `UNTRACKED`, `SNAT`, `DNAT`. Default: [] |
| **destination** string                                       | Destination specification. Address can be either a network name, a hostname, a network IP address (with /mask), or a plain IP address. Hostnames will be resolved once only, before the rule is submitted to the kernel. Please note that specifying any name to be resolved with a  remote query such as DNS is a really bad idea. The mask can be either a network mask or a plain number, specifying  the number of 1’s at the left side of the network mask. Thus, a mask of  24 is equivalent to 255.255.255.0. A `!` argument before the address specification inverts the sense of the address. |
| **destination_port** string                                  | Destination port or port range  specification. This can either be a service name or a port number. An  inclusive range can also be specified, using the format first:last. If  the first port is omitted, ‘0’ is assumed; if the last is omitted,  ‘65535’ is assumed. If the first port is greater than the second one  they will be swapped. This is only valid if the rule also specifies one  of the following protocols: tcp, udp, dccp or sctp. |
| **destination_ports** list / elements=string added in 2.11 of ansible.builtin | This specifies multiple destination port numbers or port ranges to match in the multiport module. It can only be used in conjunction with the protocols tcp, udp, udplite, dccp and sctp. |
| **dst_range** string added in 2.8 of ansible.builtin         | Specifies the destination IP range to match in the iprange module. |
| **flush** boolean added in 2.2 of ansible.builtin            | Flushes the specified table and chain of all rules. If no chain is specified then the entire table is purged. Ignores all other parameters. Choices: no ← (default) yes |
| **fragment** string                                          | This means that the rule only refers to second and further fragments of fragmented packets. Since there is no way to tell the source or destination ports of such a packet (or ICMP type), such a packet will not match any rules which  specify them. When the “!” argument precedes fragment argument, the rule will only match head fragments, or unfragmented packets. |
| **gateway** string added in 2.8 of ansible.builtin           | This specifies the IP address of host to send the cloned packets. This option is only valid when `jump` is set to `TEE`. |
| **gid_owner** string added in 2.9 of ansible.builtin         | Specifies the GID or group to use in match by owner rule.    |
| **goto** string                                              | This specifies that the processing should continue in a user specified chain. Unlike the jump argument return will not continue processing in this chain but instead in the chain that called us via jump. |
| **icmp_type** string added in 2.2 of ansible.builtin         | This allows specification of the ICMP type, which can be a numeric ICMP type, type/code pair, or one of  the ICMP type names shown by the command ‘iptables -p icmp -h’ |
| **in_interface** string                                      | Name of an interface via which a packet was received (only for packets entering the `INPUT`, `FORWARD` and `PREROUTING` chains). When the `!` argument is used before the interface name, the sense is inverted. If the interface name ends in a `+`, then any interface which begins with this name will match. If this option is omitted, any interface name will match. |
| **ip_version** string                                        | Which version of the IP protocol this rule should apply to. Choices: ipv4 ← (default) ipv6 |
| **jump** string                                              | This specifies the target of the rule; i.e., what to do if the packet matches it. The target can be a user-defined chain (other than the one this rule  is in), one of the special builtin targets which decide the fate of the  packet immediately, or an extension (see EXTENSIONS below). If this option is omitted in a rule (and the goto parameter is not  used), then matching the rule will have no effect on the packet’s fate,  but the counters on the rule will be incremented. |
| **limit** string                                             | Specifies the maximum average number of matches to allow per second. The number can specify units explicitly, using `/second`, `/minute`, `/hour` or `/day`, or parts of them (so `5/second` is the same as `5/s`). |
| **limit_burst** string added in 2.1 of ansible.builtin       | Specifies the maximum burst before the above limit kicks in. |
| **log_level** string added in 2.8 of ansible.builtin         | Logging level according to the syslogd-defined priorities. The value can be strings or numbers from 1-8. This parameter is only applicable if `jump` is set to `LOG`. Choices: 0 1 2 3 4 5 6 7 emerg alert crit error warning notice info debug |
| **log_prefix** string added in 2.5 of ansible.builtin        | Specifies a log text for the rule. Only make sense with a LOG jump. |
| **match** list / elements=string                             | Specifies a match to use, that is, an extension module that tests for a specific property. The set of matches make up the condition under which a target is invoked. Matches are evaluated first to last if specified as an array and work in short-circuit fashion, i.e. if one extension yields false,  evaluation will stop. Default: [] |
| **match_set** string added in 2.11 of ansible.builtin        | Specifies a set name which can be defined by ipset. Must be used together with the match_set_flags parameter. When the `!` argument is prepended then it inverts the rule. Uses the iptables set extension. |
| **match_set_flags** string added in 2.11 of ansible.builtin  | Specifies the necessary flags for the match_set parameter. Must be used together with the match_set parameter. Uses the iptables set extension. Choices: src dst src,dst dst,src |
| **out_interface** string                                     | Name of an interface via which a packet is going to be sent (for packets entering the `FORWARD`, `OUTPUT` and `POSTROUTING` chains). When the `!` argument is used before the interface name, the sense is inverted. If the interface name ends in a `+`, then any interface which begins with this name will match. If this option is omitted, any interface name will match. |
| **policy** string added in 2.2 of ansible.builtin            | Set the policy for the chain to the given target. Only built-in chains can have policies. This parameter requires the `chain` parameter. If you specify this parameter, all other parameters will be ignored. This parameter is used to set default policy for the given `chain`. Do not confuse this with `jump` parameter. Choices: ACCEPT DROP QUEUE RETURN |
| **protocol** string                                          | The protocol of the rule or of the packet to check. The specified protocol can be one of `tcp`, `udp`, `udplite`, `icmp`, `ipv6-icmp` or `icmpv6`, `esp`, `ah`, `sctp` or the special keyword `all`, or it can be a numeric value, representing one of these protocols or a different one. A protocol name from */etc/protocols* is also allowed. A `!` argument before the protocol inverts the test. The number zero is equivalent to all. `all` will match with all protocols and is taken as default when this option is omitted. |
| **reject_with** string added in 2.1 of ansible.builtin       | Specifies the error packet type to return while rejecting. It implies “jump: REJECT”. |
| **rule_num** string added in 2.5 of ansible.builtin          | Insert the rule as the given rule number. This works only with `action=insert`. |
| **set_counters** string                                      | This enables the administrator to initialize the packet and byte counters of a rule (during `INSERT`, `APPEND`, `REPLACE` operations). |
| **set_dscp_mark** string added in 2.1 of ansible.builtin     | This allows specifying a DSCP mark to be added to packets. It takes either an integer or hex value. Mutually exclusive with `set_dscp_mark_class`. |
| **set_dscp_mark_class** string added in 2.1 of ansible.builtin | This allows specifying a predefined DiffServ class which will be translated to the corresponding DSCP mark. Mutually exclusive with `set_dscp_mark`. |
| **source** string                                            | Source specification. Address can be either a network name, a hostname, a network IP address (with /mask), or a plain IP address. Hostnames will be resolved once only, before the rule is submitted to the kernel. Please note that specifying any name to be resolved with a  remote query such as DNS is a really bad idea. The mask can be either a network mask or a plain number, specifying  the number of 1’s at the left side of the network mask. Thus, a mask of  24 is equivalent to 255.255.255.0. A `!` argument before the address specification inverts the sense of the address. |
| **source_port** string                                       | Source port or port range specification. This can either be a service name or a port number. An inclusive range can also be specified, using the format `first:last`. If the first port is omitted, `0` is assumed; if the last is omitted, `65535` is assumed. If the first port is greater than the second one they will be swapped. |
| **src_range** string added in 2.8 of ansible.builtin         | Specifies the source IP range to match in the iprange module. |
| **state** string                                             | Whether the rule should be absent or present. Choices: absent present ← (default) |
| **syn** string added in 2.5 of ansible.builtin               | This allows matching packets that have the SYN bit set and the ACK and RST bits unset. When negated, this matches all packets with the RST or the ACK bits set. Choices: ignore ← (default) match negate |
| **table** string                                             | This option specifies the packet matching table which the command should operate on. If the kernel is configured with automatic module loading, an attempt will be made to load the appropriate module for that table if it is not already there. Choices: filter ← (default) nat mangle raw security |
| **tcp_flags** dictionary added in 2.4 of ansible.builtin     | TCP flags specification. `tcp_flags` expects a dict with the two keys `flags` and `flags_set`. Default: {} |
| **flags** list / elements=string                             | List of flags you want to examine.                           |
| **flags_set** list / elements=string                         | Flags to be set.                                             |
| **to_destination** string added in 2.1 of ansible.builtin    | This specifies a destination address to use with `DNAT`. Without this, the destination address is never altered. |
| **to_ports** string                                          | This specifies a destination port or range of ports to use, without this, the destination port is never altered. This is only valid if the rule also specifies one of the protocol `tcp`, `udp`, `dccp` or `sctp`. |
| **to_source** string added in 2.2 of ansible.builtin         | This specifies a source address to use with `SNAT`. Without this, the source address is never altered. |
| **uid_owner** string added in 2.1 of ansible.builtin         | Specifies the UID or username to use in match by owner rule. From Ansible 2.6 when the `!` argument is prepended then the it inverts the rule to apply instead to all users except that one specified. |
| **wait** string added in 2.10 of ansible.builtin             | Wait N seconds for the xtables lock to prevent multiple instances of the program from running concurrently. |



## Attributes

| Attribute      | Support         | Description                                                  |
| -------------- | --------------- | ------------------------------------------------------------ |
| **check_mode** | full            | Can run in check_mode and return changed status prediction without modifying target |
| **diff_mode**  | none            | Will return details on what has changed (or possibly needs changing in check_mode), when in diff mode |
| **platform**   | Platform: linux | Target OS/families that can be operated against              |



## Note

This module just deals with individual rules. If you need  advanced chaining of rules the recommended way is to template the  iptables restore file.



## Examples

```ini
- name: Block specific IP
  ansible.builtin.iptables:
    chain: INPUT
    source: 8.8.8.8
    jump: DROP
  become: yes

- name: Forward port 80 to 8600
  ansible.builtin.iptables:
    table: nat
    chain: PREROUTING
    in_interface: eth0
    protocol: tcp
    match: tcp
    destination_port: 80
    jump: REDIRECT
    to_ports: 8600
    comment: Redirect web traffic to port 8600
  become: yes

- name: Allow related and established connections
  ansible.builtin.iptables:
    chain: INPUT
    ctstate: ESTABLISHED,RELATED
    jump: ACCEPT
  become: yes

- name: Allow new incoming SYN packets on TCP port 22 (SSH)
  ansible.builtin.iptables:
    chain: INPUT
    protocol: tcp
    destination_port: 22
    ctstate: NEW
    syn: match
    jump: ACCEPT
    comment: Accept new SSH connections.

- name: Match on IP ranges
  ansible.builtin.iptables:
    chain: FORWARD
    src_range: 192.168.1.100-192.168.1.199
    dst_range: 10.0.0.1-10.0.0.50
    jump: ACCEPT

- name: Allow source IPs defined in ipset "admin_hosts" on port 22
  ansible.builtin.iptables:
    chain: INPUT
    match_set: admin_hosts
    match_set_flags: src
    destination_port: 22
    jump: ALLOW

- name: Tag all outbound tcp packets with DSCP mark 8
  ansible.builtin.iptables:
    chain: OUTPUT
    jump: DSCP
    table: mangle
    set_dscp_mark: 8
    protocol: tcp

- name: Tag all outbound tcp packets with DSCP DiffServ class CS1
  ansible.builtin.iptables:
    chain: OUTPUT
    jump: DSCP
    table: mangle
    set_dscp_mark_class: CS1
    protocol: tcp

# Create the user-defined chain ALLOWLIST
- iptables:
    chain: ALLOWLIST
    chain_management: true

# Delete the user-defined chain ALLOWLIST
- iptables:
    chain: ALLOWLIST
    chain_management: true
    state: absent

- name: Insert a rule on line 5
  ansible.builtin.iptables:
    chain: INPUT
    protocol: tcp
    destination_port: 8080
    jump: ACCEPT
    action: insert
    rule_num: 5

# Think twice before running following task as this may lock target system
- name: Set the policy for the INPUT chain to DROP
  ansible.builtin.iptables:
    chain: INPUT
    policy: DROP

- name: Reject tcp with tcp-reset
  ansible.builtin.iptables:
    chain: INPUT
    protocol: tcp
    reject_with: tcp-reset
    ip_version: ipv4

- name: Set tcp flags
  ansible.builtin.iptables:
    chain: OUTPUT
    jump: DROP
    protocol: tcp
    tcp_flags:
      flags: ALL
      flags_set:
        - ACK
        - RST
        - SYN
        - FIN

- name: Iptables flush filter
  ansible.builtin.iptables:
    chain: "{{ item }}"
    flush: yes
  with_items:  [ 'INPUT', 'FORWARD', 'OUTPUT' ]

- name: Iptables flush nat
  ansible.builtin.iptables:
    table: nat
    chain: '{{ item }}'
    flush: yes
  with_items: [ 'INPUT', 'OUTPUT', 'PREROUTING', 'POSTROUTING' ]

- name: Log packets arriving into an user-defined chain
  ansible.builtin.iptables:
    chain: LOGGING
    action: append
    state: present
    limit: 2/second
    limit_burst: 20
    log_prefix: "IPTABLES:INFO: "
    log_level: info

- name: Allow connections on multiple ports
  ansible.builtin.iptables:
    chain: INPUT
    protocol: tcp
    destination_ports:
      - "80"
      - "443"
      - "8081:8083"
    jump: ACCEPT
```