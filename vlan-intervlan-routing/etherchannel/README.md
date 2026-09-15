# EtherChannel / LACP Labs

## Planned Skills

- Bundle multiple physical links into one logical link
- Configure LACP
- Verify member interfaces
- Configure EtherChannel as a trunk
- Troubleshoot mismatched channel settings

## Configuration Example

```text
interface range gigabitethernet0/1-2
 channel-group 1 mode active

interface port-channel1
 switchport mode trunk
```

## Verification Commands

```text
show etherchannel summary
show interfaces port-channel 1
show interfaces trunk
show lacp neighbor
```

## Troubleshooting Checks

Member links should match on relevant Layer 2 settings such as trunk/access mode, allowed VLANs, native VLAN, speed, and duplex.
