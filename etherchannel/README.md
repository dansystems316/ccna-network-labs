# LACP EtherChannel Trunk Lab

**Status: Documentation ready.** The configuration and test plan below are examples until a project and captured outputs are added.

## Ticket and topology

A redundant switch uplink has one member suspended. Build SW1 and SW2 with two parallel physical links bundled as Port-channel1, plus PC-A on SW1 VLAN 10 and PC-B on SW2 VLAN 10. Use 192.168.10.11/24 and 192.168.10.12/24. Both member links and the logical port-channel must have consistent Layer 2 settings. Avoid adding an unrelated third path.

On **both switches** (adjust interface IDs to actual ports):

```cisco
vlan 10
interface range GigabitEthernet0/1-2
 switchport mode trunk
 switchport trunk allowed vlan 10
 channel-group 1 mode active
 no shutdown
interface Port-channel1
 switchport mode trunk
 switchport trunk allowed vlan 10
```

## Verify and fault

1. Run `show etherchannel summary` on both switches. Expect Po1 as Layer 2/in use and both physical members bundled; record the actual flags. Run `show lacp neighbor`, `show interfaces trunk`, and `show running-config interface Port-channel1` where supported. Ping PC-B from PC-A.
2. Capture a working baseline, then deliberately change one member on SW2 to `channel-group 1 mode on` while the peer remains LACP active. Confirm the member is not bundled with `show etherchannel summary`; do not claim total traffic failure if the other member still carries VLAN 10.
3. Restore `channel-group 1 mode active`, check both members and PC ping again. If Packet Tracer rejects a command or automatically resets the channel, record that result and use a supported single-member mismatch (allowed VLAN or trunk mode) instead; document the actual reason shown by IOS.

## Finish gate

Add `packet-tracer/etherchannel.pkt`, topology, SW1/SW2 text configs, before/fault/after bundle and trunk outputs, PC ping, and a short fault record showing the exact change and repair. Reopen the saved project. [Repository checklist](../PORTFOLIO_CHECKLIST.md).
