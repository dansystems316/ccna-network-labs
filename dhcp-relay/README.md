# DHCP Relay Troubleshooting

**Complete (owner verified in Packet Tracer).** The owner reopened the [final project](packet-tracer/dhcp-relay.pkt) and confirmed both clients receive DHCP leases and ping the server. Packet Tracer is unavailable in this workspace; the saved-project check is owner reported.

## Support ticket and topology

PC1 on VLAN 10 cannot obtain an address from DHCP server R2 across a routed link. Diagnose from client and switch through R1's relay and R2's pool and return route. PC2 on VLAN 20 is the unaffected control.

![Two client VLANs, relay router, and DHCP server](images/topology.png)

| Device | Connection | Address / role |
|---|---|---|
| PC1 | SW1 Fa0/1, VLAN 10 USERS | DHCP `192.168.10.21/24`, gateway `192.168.10.1` |
| PC2 | SW1 Fa0/2, VLAN 20 ADMIN | DHCP `192.168.20.21/24`, gateway `192.168.20.1` |
| SW1 | G0/1 to R1 G0/0 | 802.1Q trunk carrying VLANs 10 and 20 |
| R1 | G0/0.10; G0/0.20 | `192.168.10.1/24`; `192.168.20.1/24`; helper `10.0.12.2` on each |
| R1 | G0/1 to R2 G0/0 | `10.0.12.1/30` |
| R2 | G0/0; DHCP server | `10.0.12.2/30`; USERSPOOL and ADMINPOOL; return routes via `10.0.12.1` |

The [SW1](configs/SW1-running-config.txt), [R1](configs/R1-running-config.txt), and [R2](configs/R2-running-config.txt) configurations show the actual settings. R1 also has an unnecessary helper on unnumbered parent G0/0; the relay points for clients are the VLAN subinterfaces. The exported config preserves that observed state.

## Verification commands and evidence

| Check | Device | Command | Observed evidence |
|---|---|---|---|
| Access VLANs | SW1 | `show vlan brief` | [VLAN 10 and 20 ports](images/sw1-vlans.png) |
| Trunk | SW1 | `show interfaces trunk` | [Both VLANs forwarding](images/sw1-trunk.png) |
| Gateway interfaces | R1 | `show ip interface brief` | [Subinterfaces and server link up/up](images/r1-interfaces.png) |
| Helpers | R1 | `show running-config` | [Helpers on both VLAN interfaces](configs/R1-running-config.txt) |
| DHCP pools | R2 | `show running-config`; `show ip dhcp pool` | [Pools and exclusions](configs/R2-running-config.txt) |
| Return routes | R2 | `show ip route` | [Both VLAN routes via R1](images/r2-return-routes-installed.png) |
| Leases | R2 | `show ip dhcp binding` | [Both PC bindings](images/r2-dhcp-bindings.png) |
| Client addresses | PC1, PC2 | `ipconfig /all` | [PC1](images/pc1-lease.png), [PC2](images/pc2-lease.png) use DHCP server `10.0.12.2` |
| Gateway pings | PC1, PC2 | `ping 192.168.10.1`; `ping 192.168.20.1` respectively | [PC1](images/pc1-gateway-ping-identified.png), [PC2](images/pc2-gateway-ping-identified.png), 4/4 each |
| Server pings | PC1, PC2 | `ping 10.0.12.2` | [PC1](images/pc1-to-server-ping-identified.png), [PC2](images/pc2-to-server-ping.png), 4/4 each |

## Controlled failure and repair

The [fault record](evidence/fault-case.md) follows the [repository troubleshooting workflow](../troubleshooting/README.md): define expected behavior, scope the affected VLAN, check from client toward server, test a hypothesis, make one change, and repeat the test.

1. [Remove `ip helper-address 10.0.12.2` from R1 G0/0.10](images/fault-remove-vlan10-helper.png).
2. [PC1 DHCP fails while PC2 still receives a lease](images/fault-pc1-failed-pc2-working.png), isolating the issue to VLAN 10.
3. [Restore the helper](images/repair-restore-helper.png). This screenshot crops the interface context; the final R1 config confirms the setting on G0/0.10.
4. [PC1 regains its lease](images/repair-pc1-identified-lease.png), and the identified client pings above show server reachability.

**Root cause:** VLAN 10 DHCP broadcasts were not relayed to R2 while the helper was absent on the client gateway. Comparing helpers across VLAN gateways is a quick future check. Screenshot capture times and the method used to force the failed DHCP request were not recorded.

## Files

```text
dhcp-relay/
├── README.md
├── packet-tracer/dhcp-relay.pkt
├── configs/R1-running-config.txt
├── configs/R2-running-config.txt
├── configs/SW1-running-config.txt
├── evidence/fault-case.md
└── images/                    # topology and linked verification captures
```

This lab demonstrates fault scoping with an unaffected client, verifying the relay and return path, making one correction, and proving service from both VLANs. See the [portfolio completion criteria](../PORTFOLIO_CHECKLIST.md).
