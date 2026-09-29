# Extended ACL Rule Order Troubleshooting

## Objective

Allow PC0 and PC1 to access the server's web page. Block only ICMP echo traffic from PC0 to Server0, while allowing PC1 to ping the server. Deliberately insert an earlier permit to demonstrate that ACL entries are evaluated in order, then restore the policy.

## Topology and addressing

PC0 and PC1 connect to Switch0, which connects to Router0 `GigabitEthernet0/0`. Server0 connects to Router0 `GigabitEthernet0/1`.

| Device/interface | Address | Default gateway |
| --- | --- | --- |
| PC0 | `192.168.10.10/24` | `192.168.10.1` |
| PC1 | `192.168.10.11/24` | `192.168.10.1` |
| Router0 G0/0 | `192.168.10.1/24` | — |
| Router0 G0/1 | `192.168.20.1/24` | — |
| Server0 | `192.168.20.10/24` | `192.168.20.1` |

## Working configuration

```cisco
interface GigabitEthernet0/0
 ip address 192.168.10.1 255.255.255.0
 ip access-group SERVER_ACCESS in
 no shutdown
!
interface GigabitEthernet0/1
 ip address 192.168.20.1 255.255.255.0
 no shutdown
!
ip access-list extended SERVER_ACCESS
 10 deny icmp host 192.168.10.10 host 192.168.20.10
 20 permit ip any any
```

The ACL is inbound on the client-facing interface. Rule 10 denies PC0's ICMP traffic to this server. Rule 20 allows all other IP traffic, including PC1's ping and both clients' web access. This is a focused learning policy; a production ACL would usually define narrower permits.

## Verification and troubleshooting

| State | PC0 ping to server | PC1 ping to server | Web access |
| --- | --- | --- | --- |
| Intended policy | Fails, 100% loss | Succeeds, 0% loss | Server page loads from both PCs |
| Earlier permit added | Succeeds, 0% loss | Unchanged | Unchanged |
| Earlier permit removed | Fails again, 100% loss | Unchanged | Unchanged |

To reproduce the mistake:

```cisco
configure terminal
ip access-list extended SERVER_ACCESS
 5 permit icmp host 192.168.10.10 host 192.168.20.10
end
show ip access-lists SERVER_ACCESS
```

The router matches rule 5 first, so PC0's ping succeeds and the later deny at rule 10 is never evaluated for those packets. The screenshot of the broken ACL shows prior matches on rule 10 from before rule 5 was inserted; those accumulated counts do not prove the deny was still matching afterward.

Restore the intended policy:

```cisco
configure terminal
ip access-list extended SERVER_ACCESS
 no 5
end
show ip access-lists SERVER_ACCESS
```

The final PC0 ping again received four `Destination host unreachable` replies from `192.168.10.1` and reported 100% loss. The final `show ip access-lists SERVER_ACCESS` screenshot shows only the deny and permit entries, with no earlier rule 5. The deny match count increased from 8 to 12 after four additional probes. The router interface screenshot showed G0/0 and G0/1 both `up/up`.

## Evidence

| Finding | Screenshot |
| --- | --- |
| Topology | [Topology](evidence/topology.png) |
| Both router interfaces up/up | [Router interfaces](evidence/router-interfaces.png) |
| PC0 blocked by intended ACL | [PC0 failed ping](evidence/pc0-ping-blocked.png) |
| PC0 can access HTTPS on Server0 | [PC0 browser](evidence/pc0-https-allowed.png) |
| PC1 ping allowed | [PC1 successful ping](evidence/pc1-ping-allowed.png) |
| Incorrect early permit | [Broken ACL](evidence/acl-rule-order-broken.png) |
| PC0 ping succeeds during mistake | [PC0 successful ping](evidence/pc0-ping-incorrectly-allowed.png) |
| Rule 5 removed and deny matches increase from 8 to 12 | [Restored ACL](evidence/acl-restored-match-counts.png) |
| PC0 ping fails again | [PC0 failed ping after fix](evidence/pc0-ping-blocked-after-fix.png) |

Open [the Packet Tracer lab](acl-troubleshooting-final.pkt) to reproduce the tests. The browser evidence uses HTTPS, so it demonstrates permitted web access over HTTPS rather than HTTP specifically.
