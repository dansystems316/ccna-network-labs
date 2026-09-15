# Spanning Tree Labs

## Planned Skills

- Elect the STP root bridge
- Identify root, designated, and alternate ports
- Configure Rapid PVST+
- Set root primary / secondary
- Configure PortFast and BPDU Guard
- Troubleshoot blocked ports and unexpected root elections

## Core Commands

```text
show spanning-tree
show spanning-tree vlan 10
show spanning-tree root
show spanning-tree interface <interface> detail
```

## Configuration Examples

```text
spanning-tree mode rapid-pvst
spanning-tree vlan 10 root primary

interface range fastethernet0/1-20
 spanning-tree portfast
 spanning-tree bpduguard enable
```

## Verification Goal

Document the elected root bridge, each switch's root port, forwarding/blocking state, and how the topology reconverges after a link failure.
