# Lab Index

Status definitions:

- **Complete:** reproducible lab with source file, topology, configurations, verification output, and troubleshooting evidence.
- **In progress:** genuine artifacts exist but the evidence set is incomplete.
- **Documentation ready:** technical plan and commands are documented; lab artifacts still need to be added.
- **Planned:** outline exists, but the lab has not yet been evidenced.

| Priority | Lab | Role relevance | Current status | Evidence needed next |
|---:|---|---|---|---|
| 1 | [VLAN and inter-VLAN routing](vlan-intervlan-routing/) | Core switching and gateway support | In progress | Add text configs, trunk output, and captured PC1-to-PC2 ping; `.pkt`, topology, VLAN/router state, and access-VLAN fault evidence are included |
| 2 | [OSPF single area](ospf/) | Junior routing and escalation work | In progress | Add text running configurations; `.pkt`, topology and neighbor/route/ping/fault images are present |
| 3 | [DHCP relay troubleshooting](dhcp-relay/) | Common junior support address-assignment ticket | In progress | `.pkt`, topology, VLAN/trunk, R1 helpers, both client leases, matching server bindings, current pool config, installed return routes, and cropped server/VLAN 10 gateway pings present; capture attributed gateway/server pings from both PCs, saved configs and genuine fault/repair proof |
| 4 | [Spanning Tree](spanning-tree/) | Loop prevention and switch troubleshooting | Planned | Three-switch redundant topology, root/port-role output, link-failure reconvergence |
| 5 | [EtherChannel](etherchannel/) | Uplink resiliency and bandwidth | Planned | LACP bundle, member-state output, deliberate mismatch and repair |
| 6 | [ACLs](acl/) | Network access policy and troubleshooting | Planned | Written policy matrix, configs, permit/deny tests, hit counters |
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
