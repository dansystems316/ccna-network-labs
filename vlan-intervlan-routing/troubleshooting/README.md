# Network Troubleshooting

This section contains the workflow used across the labs and will hold evidence-based incident records. A troubleshooting case is not considered complete without genuine before-and-after output.

## Troubleshooting Workflow

1. Define one exact failure.
2. Record the expected behavior.
3. Establish the scope: one host, one VLAN, one path, or the entire network.
4. Test from the nearest point to the farthest point.
5. Compare expected state with actual device output.
6. Form a specific hypothesis.
7. Make one controlled change.
8. Repeat the original test and capture proof.

## Layered Checks

| Area | Questions | Useful commands |
|---|---|---|
| Physical/interface | Is the interface present, enabled, and up/up? | `show interfaces`, `show ip interface brief` |
| Layer 2 | Is the port in the right VLAN? Is the VLAN carried and forwarding? | `show vlan brief`, `show interfaces trunk`, `show spanning-tree`, `show mac address-table` |
| Link aggregation | Did all expected members join the bundle? | `show etherchannel summary`, `show lacp neighbor` |
| Layer 3 | Are address, mask, gateway, ARP, and routes correct? | `show ip interface brief`, `show arp`, `show ip route` |
| OSPF | Are neighbors full and networks advertised? | `show ip ospf neighbor`, `show ip ospf interface brief`, `show ip protocols` |
| Policy/NAT | Is traffic denied or failing translation? | `show access-lists`, `show ip interface`, `show ip nat translations`, `show ip nat statistics` |
| Access security | Is DHCP or ARP inspection dropping valid traffic? | `show ip dhcp snooping`, `show ip dhcp snooping binding`, `show ip arp inspection statistics` |
| IPv6 | Are prefixes, neighbors, and routes correct? | `show ipv6 interface brief`, `show ipv6 neighbors`, `show ipv6 route` |

## Progressive Testing

Test from the closest dependency outward:

```text
local TCP/IP stack
local interface
default gateway
next hop
remote gateway
remote host or service
```

Use ping, traceroute, ARP/neighbor tables, and device state to identify the first failing boundary.

## Case Index

| Case | Lab | Symptom | Root cause | Evidence |
|---|---|---|---|---|
| Pending | VLAN | Host cannot reach its gateway | To be confirmed in a real lab | Before/after output pending |
| Pending | OSPF | Neighbor table is empty | To be confirmed in a real lab | Before/after output pending |
| Pending | NAT/PAT | No translations appear | To be confirmed in a real lab | Before/after output pending |

Replace each pending row with an actual case after reproducing and fixing the fault. Good candidates from hands-on practice include a wrong access VLAN, trunk allowed-VLAN omission, OSPF subnet-mask mismatch, EtherChannel inconsistency, ACL direction error, missing NAT inside/outside designation, or incorrect DHCP trust boundary.

## Incident Record Template

Use [the lab template](../templates/lab-template.md) or create a separate case file containing:

1. Ticket or symptom
2. Scope and user impact
3. Expected behavior
4. Initial command output
5. Hypothesis
6. Tests and observations
7. Confirmed root cause
8. Corrective change
9. Post-fix output
10. Prevention or escalation note
