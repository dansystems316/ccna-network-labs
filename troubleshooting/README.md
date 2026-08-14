# Network Troubleshooting Workflow

A repeatable troubleshooting process for Cisco labs and real networks.

## 1. Define the Failure

Identify exactly what does not work.

Examples:
- One host cannot reach its gateway.
- Hosts in one VLAN cannot reach another VLAN.
- An OSPF neighbor is missing.
- A trunk is not carrying a VLAN.
- A route exists but traffic is still blocked.

Avoid starting with random configuration changes.

## 2. Check Layer 1

```text
show ip interface brief
show interfaces
```

Check:
- Interface status
- Line protocol
- Cabling
- Shutdown state
- Speed/duplex issues when relevant

## 3. Check Layer 2

```text
show vlan brief
show interfaces trunk
show interfaces switchport
show mac address-table
show spanning-tree
show etherchannel summary
```

Check:
- Correct access VLAN
- Trunk state
- Allowed VLANs
- Native VLAN
- STP forwarding/blocking state
- EtherChannel membership
- MAC learning

## 4. Check Layer 3

```text
show ip interface brief
show ip route
show arp
show ip protocols
```

Check:
- IP address
- Subnet mask
- Default gateway
- Connected routes
- Static routes
- Dynamic routes

## 5. Check Routing Protocols

### OSPF

```text
show ip ospf neighbor
show ip ospf interface brief
show ip protocols
show ip route ospf
```

Check:
- Neighbor state
- Area number
- Network statements
- Timers
- Router IDs
- Passive interfaces

## 6. Check Security and Policy

```text
show access-lists
show ip interface
show port-security interface
show ip dhcp snooping
show ip arp inspection
```

An otherwise correct route may still fail because policy blocks the traffic.

## 7. Test Progressively

Test from nearest to farthest:

```text
ping 127.0.0.1
ping <local-interface>
ping <default-gateway>
ping <next-hop>
ping <remote-host>
traceroute <remote-host>
```

This helps identify where forwarding stops.

## 8. Compare Expected vs. Actual State

For every problem, write down:

- What should happen?
- What is actually happening?
- Which command proves the difference?

## 9. Make One Change at a Time

After each change, re-run the verification command that exposed the fault.

## Core Cisco Verification Commands

```text
show running-config
show startup-config
show ip interface brief
show interfaces status
show vlan brief
show interfaces trunk
show mac address-table
show spanning-tree
show etherchannel summary
show ip route
show arp
show cdp neighbors detail
show lldp neighbors detail
show access-lists
show ip ospf neighbor
show ip protocols
```

## Documentation Standard

When recording a troubleshooting case, capture:

1. Symptom
2. Expected behavior
3. Commands used
4. Relevant output
5. Root cause
6. Configuration change
7. Verification after the fix
8. Lesson learned
