# Access Control List Labs

## Planned Skills

- Standard IPv4 ACLs
- Extended IPv4 ACLs
- Numbered and named ACLs
- Correct ACL placement
- Verify permit/deny matches
- Troubleshoot implicit deny behavior

## Verification Commands

```text
show access-lists
show ip interface
show running-config | section access-list
```

## Example Extended ACL

```text
ip access-list extended USERS_TO_WEB
 permit tcp 192.168.10.0 0.0.0.255 host 192.168.30.10 eq 80
 permit tcp 192.168.10.0 0.0.0.255 host 192.168.30.10 eq 443
 deny ip 192.168.10.0 0.0.0.255 192.168.30.0 0.0.0.255
 permit ip any any

interface gigabitethernet0/0
 ip access-group USERS_TO_WEB in
```

## Documentation Goal

For each ACL lab, record the intended policy before writing the ACL, then prove allowed and denied traffic with tests and ACL hit counters.
