# Lab Index

**Complete** means a saved Packet Tracer project, device configs, topology, actual verification and a demonstrated failure/repair. **In progress** means some real artifacts exist. **Documentation ready** means the build and evidence plan exists, with lab work still to do. See the [fast finish plan](PORTFOLIO_FINISH_PLAN.md) and [completion checklist](PORTFOLIO_CHECKLIST.md).

| Priority | Lab | Status | Next proof to capture |
|---:|---|---|---|
| 1 | [VLAN and inter-VLAN routing](vlan-intervlan-routing/) | In progress | Sanitized text configs and an identified PC1-to-PC2 ping; confirm trunk output is already present before making another copy |
| 2 | [OSPF single area](ospf/) | In progress | R1/R2/R3 text configs; compare with neighbor, route, ping and mask-fault images |
| 3 | [DHCP relay troubleshooting](dhcp-relay/) | Complete (owner verified) | None required; final saved project and actual troubleshooting evidence present |
| 4 | [Rapid PVST+](spanning-tree/) | Documentation ready | Three-switch triangle, root and alternate output, failover or wrong-root repair |
| 5 | [LACP EtherChannel](etherchannel/) | Documentation ready | Bundled members, mismatch and repaired member state |
| 6 | [Extended ACL](acl/) | Documentation ready | Policy matrix, real HTTP/ICMP allow-deny tests, wrong-rule repair |
| 7 | [PAT](nat-pat/) | Documentation ready | Routing baseline, translations, inside-role failure and repair |
| 8 | [DHCP Snooping and DAI](dhcp-snooping-dai/) | Documentation ready | Actual bindings, trust-boundary failure and repair, supported DAI check |
| 9 | [IPv6 static routing](ipv6-routing/) | Documentation ready | Two-way routes, identified PC ping, broken return route and fix |

A capstone can integrate these skills later, after the smaller cases have actual working Packet Tracer files and evidence. Do not count an example configuration as a tested lab.
