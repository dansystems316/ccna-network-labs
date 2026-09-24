# CCNA Network Labs Portfolio

Hands-on Cisco networking portfolio focused on configuration, verification, and structured troubleshooting for junior network and IT infrastructure roles.

> **Current status:** DHCP relay is complete (owner verified); VLAN and OSPF have real artifacts but need final evidence. Six additional labs have build and verification playbooks, with testing still pending.

## What This Portfolio Demonstrates

- Building segmented networks with VLANs, trunks, and inter-VLAN routing
- Establishing and validating IPv4 and IPv6 connectivity
- Configuring OSPF, EtherChannel, STP, ACLs, NAT/PAT, and Layer 2 security
- Isolating DHCP address failures across routed VLANs
- Using Cisco IOS evidence to prove expected behavior
- Diagnosing faults from symptoms instead of making random configuration changes
- Recording root cause, corrective action, and post-fix validation

## Start Here

- [Lab index and completion status](LAB_INDEX.md)
- [Fast portfolio finish plan](PORTFOLIO_FINISH_PLAN.md)
- [Troubleshooting cases and workflow](troubleshooting/README.md)
- [Evidence checklist](PORTFOLIO_CHECKLIST.md)
- [Reusable lab template](templates/lab-template.md)

## Featured Labs

| Lab | Skills shown | Evidence status |
|---|---|---|
| [VLAN and inter-VLAN routing](vlan-intervlan-routing/) | VLANs, access ports, 802.1Q trunks, router-on-a-stick | In progress; Packet Tracer lab and troubleshooting evidence included |
| [OSPF single area](ospf/) | Neighbors, route learning, router IDs, adjacency troubleshooting | In progress; Packet Tracer file and verification/fault images included; running configurations pending |
| [DHCP relay troubleshooting](dhcp-relay/) | Client address failure, relay placement, server pools, return path | Complete; final Packet Tracer file, configs, client tests, and failure/repair evidence |
| [Spanning Tree](spanning-tree/) | Rapid PVST+, root election, PortFast, BPDU Guard | Documentation ready; Packet Tracer build and evidence pending |
| [EtherChannel](etherchannel/) | LACP, port-channel trunks, consistency checks | Documentation ready; Packet Tracer build and evidence pending |
| [Access control lists](acl/) | Standard/extended ACLs, placement, hit counters | Documentation ready; Packet Tracer build and evidence pending |
| [NAT and PAT](nat-pat/) | Inside/outside roles, overload, translation checks | Documentation ready; Packet Tracer build and evidence pending |
| [DHCP Snooping and DAI](dhcp-snooping-dai/) | Trust boundaries, bindings, ARP inspection | Documentation ready; Packet Tracer build and evidence pending |
| [IPv6 routing](ipv6-routing/) | Addressing, neighbor discovery, static/default routes | Documentation ready; Packet Tracer build and evidence pending |

A lab will be marked **complete** only when it includes the topology, addressing plan, Packet Tracer file, configurations, verification output, and at least one documented troubleshooting case.

## Troubleshooting Method

1. Define the exact symptom and expected behavior.
2. Test from the nearest point to the farthest point.
3. Inspect Layer 1, Layer 2, Layer 3, routing, and policy in order.
4. Record the command output that exposes the fault.
5. Make one controlled change.
6. Repeat the original test and capture proof of recovery.

## Tools

- Cisco Packet Tracer
- Cisco IOS CLI
- Git and GitHub

## Repository Structure

```text
ccna-network-labs/
├── LAB_INDEX.md
├── PORTFOLIO_FINISH_PLAN.md
├── PORTFOLIO_CHECKLIST.md
├── vlan-intervlan-routing/
├── ospf/
├── dhcp-relay/
├── spanning-tree/
├── etherchannel/
├── acl/
├── nat-pat/
├── dhcp-snooping-dai/
├── ipv6-routing/
├── troubleshooting/
└── templates/
```

## Next Build

Finish the existing VLAN and OSPF lab exports, then follow the [fast finish plan](PORTFOLIO_FINISH_PLAN.md) for STP and the remaining small troubleshooting cases. A larger capstone can follow once the focused labs have real evidence.

## About

Built by Dan Partain as practical evidence of Cisco networking and troubleshooting skills while preparing for a junior network role.
