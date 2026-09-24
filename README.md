# CCNA Network Labs Portfolio

Hands-on Cisco networking portfolio focused on configuration, verification, and structured troubleshooting for junior network and IT infrastructure roles.

> **Current status:** The repository contains documented lab foundations. Packet Tracer files, topology screenshots, saved configurations, and real verification output are being added before individual labs are marked complete.

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
- [Troubleshooting cases and workflow](troubleshooting/README.md)
- [Evidence checklist](PORTFOLIO_CHECKLIST.md)
- [Reusable lab template](templates/lab-template.md)

## Featured Labs

| Lab | Skills shown | Evidence status |
|---|---|---|
| [VLAN and inter-VLAN routing](vlan-intervlan-routing/) | VLANs, access ports, 802.1Q trunks, router-on-a-stick | In progress; Packet Tracer lab and troubleshooting evidence included |
| [OSPF single area](ospf/) | Neighbors, route learning, router IDs, adjacency troubleshooting | In progress; Packet Tracer file and verification/fault images included; running configurations pending |
| [DHCP relay troubleshooting](dhcp-relay/) | Client address failure, relay placement, server pools, return path | In progress; Packet Tracer project, topology and baseline IOS screenshots included; lease and failure/recovery proof pending |
| [Spanning Tree](spanning-tree/) | Rapid PVST+, root election, PortFast, BPDU Guard | Planned lab; artifacts pending |
| [EtherChannel](etherchannel/) | LACP, port-channel trunks, consistency checks | Planned lab; artifacts pending |
| [Access control lists](acl/) | Standard/extended ACLs, placement, hit counters | Planned lab; artifacts pending |
| [NAT and PAT](nat-pat/) | Inside/outside roles, overload, translation checks | Planned lab; artifacts pending |
| [DHCP Snooping and DAI](dhcp-snooping-dai/) | Trust boundaries, bindings, ARP inspection | Planned lab; artifacts pending |
| [IPv6 routing](ipv6-routing/) | Addressing, neighbor discovery, static/default routes | Planned lab; artifacts pending |

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
- Wireshark
- Linux networking tools
- Git and GitHub

## Repository Structure

```text
ccna-network-labs/
├── LAB_INDEX.md
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

The highest-value next addition is a small-enterprise capstone combining VLAN segmentation, DHCP, inter-VLAN routing, OSPF, ACL policy, NAT/PAT, Layer 2 protections, device management, and monitoring. That project will show how the individual technologies work together in an operational network.

## About

Built by Dan Partain as practical evidence of Cisco networking and troubleshooting skills while preparing for a junior network role.
