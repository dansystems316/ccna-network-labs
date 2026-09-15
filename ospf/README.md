# OSPF Single-Area Lab

## Objective

Configure OSPFv2 in area 0 between three routers, verify neighbor adjacencies, and confirm that each router learns remote networks dynamically.

## Example Topology

```text
LAN-A -- R1 ---- R2 ---- R3 -- LAN-C
              |
            LAN-B
```

## Example Addressing

| Device | Interface | Address |
|---|---|---|
| R1 | G0/0 | 192.168.10.1/24 |
| R1 | G0/1 | 10.0.12.1/30 |
| R2 | G0/0 | 10.0.12.2/30 |
| R2 | G0/1 | 10.0.23.1/30 |
| R2 | G0/2 | 192.168.20.1/24 |
| R3 | G0/0 | 10.0.23.2/30 |
| R3 | G0/1 | 192.168.30.1/24 |

## Configuration

### R1

```text
router ospf 1
 router-id 1.1.1.1
 network 192.168.10.0 0.0.0.255 area 0
 network 10.0.12.0 0.0.0.3 area 0
```

### R2

```text
router ospf 1
 router-id 2.2.2.2
 network 10.0.12.0 0.0.0.3 area 0
 network 10.0.23.0 0.0.0.3 area 0
 network 192.168.20.0 0.0.0.255 area 0
```

### R3

```text
router ospf 1
 router-id 3.3.3.3
 network 10.0.23.0 0.0.0.3 area 0
 network 192.168.30.0 0.0.0.255 area 0
```

## Verification

```text
show ip ospf neighbor
show ip route ospf
show ip protocols
show ip ospf interface brief
```

Expected result:
- Adjacent routers reach FULL state.
- Remote networks appear in the routing table with an `O` code.
- End hosts can reach remote LANs.

## Troubleshooting Checklist

If a neighbor does not form:

1. Confirm both interfaces are up/up.
2. Verify IP addresses and subnet masks.
3. Confirm both interfaces are in the same IP subnet.
4. Verify OSPF is enabled on the interfaces.
5. Verify both interfaces are in the same OSPF area.
6. Check hello/dead timers.
7. Check for passive interfaces.
8. Check authentication if configured.
9. Check duplicate router IDs.

Useful commands:

```text
show ip interface brief
show running-config | section router ospf
show ip ospf interface
show ip ospf neighbor
show ip protocols
```

## Example Fault: Subnet Mask Mismatch

A physical link can be up while OSPF adjacency still fails because the two router interfaces do not agree on the Layer 3 subnet.

Check both sides carefully:

```text
show ip interface brief
show running-config interface gigabitethernet0/0
```

Correct the addressing, then re-check the neighbor table.

## What This Lab Demonstrates

- OSPF process configuration
- Router IDs
- Wildcard masks
- Area 0 operation
- Neighbor verification
- Dynamic route verification
- Systematic adjacency troubleshooting
