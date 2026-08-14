# NAT / PAT Labs

## Planned Skills

- Static NAT
- Dynamic NAT pools
- PAT / overload
- Identify inside and outside interfaces
- Verify translations and counters

## Example PAT Configuration

```text
access-list 1 permit 192.168.10.0 0.0.0.255

interface gigabitethernet0/0
 ip nat inside

interface gigabitethernet0/1
 ip nat outside

ip nat inside source list 1 interface gigabitethernet0/1 overload
```

## Verification Commands

```text
show ip nat translations
show ip nat statistics
show access-lists
```

## Troubleshooting Checks

Verify routing first, then confirm inside/outside interface roles, the source ACL, and the NAT rule. Clear old translations when testing changed configurations if necessary.
