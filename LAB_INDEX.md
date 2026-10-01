# Lab Index

Status definitions:

- **Complete:** reproducible lab with source file, topology, configurations, verification output, and troubleshooting evidence.
- **In progress:** genuine artifacts exist but the evidence set is incomplete.
- **Documentation ready:** technical plan and commands are documented; lab artifacts still need to be added.
- **Planned:** outline exists, but the lab has not yet been evidenced.

| Priority | Lab | Role relevance | Current status | Evidence needed next |
|---:|---|---|---|---|
| 1 | [VLAN and inter-VLAN routing](vlan-intervlan-routing/) | Core switching and gateway support | In progress | Add text configs and review the uploaded trunk/ping screenshots; `.pkt`, topology, VLAN/router state, and access-VLAN failure/repair evidence are included |
| 2 | [OSPF single area](ospf/) | Junior routing and escalation work | Complete | Original project, topology/addressing, three reviewed running configs, neighbor/route/ping evidence and mask-mismatch failure/repair case included; [supplemental captures](ospf/three-router-verification/) retain their own interface mapping |
| 3 | [DHCP relay troubleshooting](dhcp-relay/) | Common junior support address-assignment ticket | Complete | Final `.pkt` retested after reopening by owner; three running configs, VLAN/trunk and lease evidence, identified client pings, and VLAN 10 helper failure/repair with unaffected VLAN 20 control |
| 4 | [Spanning Tree](spanning-tree/) | Loop prevention and switch troubleshooting | In progress | `.pkt`, topology, root/blocked-port output and post-failover ping are included; add switch config exports and restoration check. Captured protocol is classic 802.1D STP |
| 5 | [EtherChannel](etherchannel/) | Uplink resiliency and bandwidth | In progress | Project, switch summaries, normal/failover pings and mismatch repair images are included; add both switch config exports and a clearly linked topology |
| 6 | [ACL rule-order troubleshooting](acl-troubleshooting/) | Network access policy and troubleshooting | In progress | Project, addressing/policy table, config excerpt, permit/deny tests, counters and failure/repair screenshots are included; add full router config export |
| 7 | [NAT/PAT](nat-pat/) | Internet edge troubleshooting | Planned | Inside/outside topology, routes, translations, statistics, no-translation fault |
| 8 | [DHCP Snooping and DAI](dhcp-snooping-dai/) | Access-layer security | Planned | DHCP server/client topology, binding table, rogue-server or trust-port test |
| 9 | [IPv6 routing](ipv6-routing/) | Modern addressing and routing | Planned | Dual-router lab, neighbor/route output, ping/traceroute, broken-prefix case |

## Highest-Value Missing Labs

These additions would strengthen the portfolio most for a junior network role:

| Priority | Proposed lab | Why recruiters and hiring managers care |
|---:|---|---|
| 1 | Small-enterprise capstone | Proves multiple technologies can be integrated and troubleshot together |
| 2 | DNS troubleshooting | Closely matches common support tickets after address assignment succeeds |
| 3 | Secure device management | Shows SSH, local users, privilege control, banners, and management-plane basics |
| 4 | Network monitoring and time | Demonstrates NTP, Syslog, SNMP, CDP/LLDP, and evidence collection |
| 5 | Switch access security | Adds port security, unused-port shutdown, PortFast, and BPDU Guard |
| 6 | Configuration backup and recovery | Shows TFTP/SCP backup, restore, startup/running configuration, and change discipline |

## Recommended Capstone

Build one realistic branch-office network with:

- User, Admin, Server, Guest, and Management VLANs
- Redundant access/distribution switching
- Rapid PVST+ and LACP uplinks
- Layer 3 switching or router-on-a-stick
- DHCP pools and DHCP relay
- Single-area OSPF between two routing devices
- ACLs enforcing a written traffic policy
- PAT for simulated internet access
- DHCP Snooping, DAI, PortFast, and BPDU Guard
- SSH management plus NTP, Syslog, SNMP, CDP, and LLDP
- IPv6 addressing and routing
- Five injected faults with before/after evidence

That capstone should be the featured project at the top of the repository once complete.
