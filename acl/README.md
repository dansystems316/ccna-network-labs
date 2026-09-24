# Extended ACL Policy and Troubleshooting Lab

**Status: Documentation ready.** No device test results are implied by the example policy.

## Ticket, topology, and policy

Users must reach a server's web service but must not reach its other IP services. Admin clients must still reach the server. Use one switch, R1 with VLAN 10 USERS and VLAN 20 ADMIN subinterfaces and a separate VLAN 30 SERVER subinterface; attach one PC to each client VLAN and a server to VLAN 30. Configure 802.1Q trunk and matching VLAN access ports. Static example: PC-U 192.168.10.10/24 GW .10.1; PC-A 192.168.20.10/24 GW .20.1; server 192.168.30.10/24 GW .30.1. Enable the server's HTTP service.

| Source | Destination | Expected |
|---|---|---|
| USERS | Server TCP/80 | Permit |
| USERS | Server ICMP | Deny |
| ADMIN | Server TCP/80 and ICMP | Permit |
| USERS | Other destinations | Permit for this scoped lab |

R1 example, applied **inbound on VLAN 10's subinterface**:

```cisco
ip access-list extended USERS_SERVER_POLICY
 permit tcp 192.168.10.0 0.0.0.255 host 192.168.30.10 eq 80
 deny ip 192.168.10.0 0.0.0.255 host 192.168.30.10
 permit ip 192.168.10.0 0.0.0.255 any
interface GigabitEthernet0/0.10
 ip access-group USERS_SERVER_POLICY in
```

## Verify and fault

1. First prove basic routes and service without the ACL: PC-U HTTP to `http://192.168.30.10`, ping server, and PC-A ping/HTTP. Capture R1 `show ip interface brief` and switch `show vlan brief`, `show interfaces trunk`.
2. Apply the ACL; retest the four policy rows. Run `show access-lists USERS_SERVER_POLICY` and `show ip interface GigabitEthernet0/0.10` to check counters and direction. Packet Tracer hit counters may differ by version; write down observed output rather than assuming counts.
3. Inject one mistake: remove the TCP/80 permit with `no permit tcp 192.168.10.0 0.0.0.255 host 192.168.30.10 eq 80`. Confirm USER HTTP now fails while ADMIN HTTP still works; restore the permit **above the deny** using sequence numbers if supported, otherwise rebuild the ACL in the correct order. Retest and record exact IOS output.

## Finish gate

Add final `.pkt`, topology, R1/SW1 configs, policy matrix with actual pass/fail observations, command output and a before/fault/after case. Do not claim HTTP is permitted based on ping; use the PC web browser. [Repository checklist](../PORTFOLIO_CHECKLIST.md).
