# CCNA Network Labs

Hands-on Cisco networking labs documenting configuration, verification, and troubleshooting practice for CCNA-level networking.

## Purpose

This repository is a practical networking portfolio. Each lab is designed to show not only the final configuration, but also how the network was verified and how faults were diagnosed.

## Lab Areas

- VLANs and inter-VLAN routing
- 802.1Q trunks and native VLANs
- STP / Rapid PVST+
- EtherChannel / LACP
- OSPF
- IPv4 and IPv6 static routing
- Standard and extended ACLs
- NAT / PAT
- DHCP and DHCP relay
- DHCP Snooping and Dynamic ARP Inspection
- Port security
- NTP, Syslog, SNMP, CDP, and LLDP
- Wireless fundamentals
- Network troubleshooting with Cisco IOS and Wireshark

## Verification Commands

```text
show ip interface brief
show interfaces trunk
show vlan brief
show spanning-tree
show etherchannel summary
show ip route
show ip ospf neighbor
show ip protocols
show access-lists
show ip nat translations
show cdp neighbors
show lldp neighbors
```

## Repository Structure

```text
ccna-network-labs/
├── vlan-intervlan-routing/
├── ospf/
├── spanning-tree/
├── etherchannel/
├── acl/
├── nat-pat/
├── dhcp-snooping-dai/
├── ipv6-routing/
├── troubleshooting/
└── templates/
```

## Lab Documentation Standard

Each completed lab should contain:

1. Objective
2. Topology
3. Addressing table
4. Configuration
5. Verification
6. Problem encountered
7. Troubleshooting process
8. Resolution
9. What I learned

## Tools

- Cisco Packet Tracer
- Cisco IOS
- Wireshark
- Linux command-line networking tools
- Git and GitHub

## Current Goal

Build a documented collection of repeatable networking labs that demonstrates practical Cisco configuration and troubleshooting skills.
