# VLAN and Inter-VLAN Routing Lab

## Objective

Create multiple VLANs, configure switch access ports and an 802.1Q trunk, then provide inter-VLAN routing with router-on-a-stick.

## Example Topology

```text
PC1 ---- SW1 ---- R1
          |
PC2 ------+
```

- PC1: VLAN 10
- PC2: VLAN 20
- SW1 to R1: 802.1Q trunk

## Addressing

| Device | Interface | Address | Purpose |
|---|---|---|---|
| R1 | G0/0.10 | 192.168.10.1/24 | VLAN 10 gateway |
| R1 | G0/0.20 | 192.168.20.1/24 | VLAN 20 gateway |
| PC1 | NIC | 192.168.10.10/24 | VLAN 10 host |
| PC2 | NIC | 192.168.20.10/24 | VLAN 20 host |

## Switch Configuration

```text
enable
configure terminal

vlan 10
 name USERS
vlan 20
 name ADMIN

interface fastethernet0/1
 switchport mode access
 switchport access vlan 10

interface fastethernet0/2
 switchport mode access
 switchport access vlan 20

interface gigabitethernet0/1
 switchport mode trunk
 switchport trunk allowed vlan 10,20

end
write memory
```

## Router Configuration

```text
enable
configure terminal

interface gigabitethernet0/0
 no shutdown

interface gigabitethernet0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0

interface gigabitethernet0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0

end
write memory
```

## Verification

### Switch

```text
show vlan brief
show interfaces trunk
show interfaces switchport
```

### Router

```text
show ip interface brief
show running-config interface gigabitethernet0/0.10
show running-config interface gigabitethernet0/0.20
```

### End-to-End Test

From PC1:

```text
ping 192.168.10.1
ping 192.168.20.1
ping 192.168.20.10
```

## Common Failure Scenarios

### Wrong access VLAN

Symptoms:
- Host cannot reach its default gateway.
- `show vlan brief` places the port in the wrong VLAN.

Fix:

```text
interface fastethernet0/1
 switchport access vlan 10
```

### VLAN missing from trunk

Symptoms:
- Local devices in the VLAN work, but traffic cannot cross the trunk.

Check:

```text
show interfaces trunk
```

Fix the allowed VLAN list if needed.

### Incorrect subinterface VLAN tag

Symptoms:
- Router subinterface is configured but the VLAN still cannot reach its gateway.

Check that the VLAN ID in `encapsulation dot1Q` matches the switch VLAN.

## What This Lab Demonstrates

- VLAN creation
- Access port assignment
- 802.1Q trunking
- Router subinterfaces
- Default gateway configuration
- Layer 2 vs. Layer 3 troubleshooting
