# PAT Internet Edge Troubleshooting Lab

**Status: Documentation ready.** Addressing and outputs below are a build plan.

## Ticket and design

A LAN host can reach its default gateway but cannot reach a simulated outside server. Build PC1—SW1—R1—R2—Server. Use PC1 `192.168.10.10/24` (GW `.10.1`), R1 inside `192.168.10.1/24`, R1 outside `203.0.113.1/30`, R2 link `203.0.113.2/30`, R2 server interface `198.51.100.1/24`, and server `198.51.100.10/24` (GW `.100.1`). These are documentation/example ranges. Add R1 default route via `203.0.113.2`. R2 only needs the directly connected networks for translated return traffic.

R1 example (adjust actual interface IDs):

```cisco
access-list 1 permit 192.168.10.0 0.0.0.255
interface GigabitEthernet0/0
 ip nat inside
interface GigabitEthernet0/1
 ip nat outside
ip nat inside source list 1 interface GigabitEthernet0/1 overload
ip route 0.0.0.0 0.0.0.0 203.0.113.2
```

## Verify and fault

1. Check `show ip interface brief`, `show ip route`, `show access-lists 1`, `show ip nat statistics`. Ping R1 inside from PC1 and ping the server from R1 to isolate routing before NAT.
2. From PC1, ping `198.51.100.10`; then run R1 `show ip nat translations` and `show ip nat statistics`. Record inside local/global and outside address and real translation counters. A ping alone does not prove a translation.
3. After capturing a working baseline, remove `ip nat inside` from the R1 LAN interface. Repeat the PC test and inspect translations/statistics; restore `ip nat inside` and repeat. Old entries may remain briefly: note their age or use `clear ip nat translation *` only after documenting the old state, if supported by the image.

## Finish gate

Add final `.pkt`, topology, R1/R2 configs, routes, baseline/fault/repaired PC tests and PAT output, and a fault case explaining the failed boundary. Reopen and retest saved project. [Repository checklist](../PORTFOLIO_CHECKLIST.md).
