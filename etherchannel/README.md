# EtherChannel / LACP troubleshooting

## Goal

Bundle two links between Cisco 2960 switches using LACP. CDP identified Fa0/2 ↔ Fa0/2 and Fa0/3 ↔ Fa0/3. The trunk carries VLAN 10 with native VLAN 99.

## Fault and fix

The channel was initially down: `Po1(SD)` with standalone or suspended members. The switch reported incompatible DTP settings and allowed VLANs. One switch also lacked `switchport mode trunk` on its member ports.

Both ends were set to trunk mode with native VLAN 99, allowed VLAN 10, and LACP `active`. Fa0/2 then joined while Fa0/3 remained standalone or suspended. Removing and re-adding Fa0/3 to channel-group 1 on both switches produced this final observed result:

```text
1      Po1(SU)           LACP   Fa0/2(P) Fa0/3(P)
```

`SU` means the Layer 2 channel is in use; `P` means a physical port is bundled. VLAN 99 was also created on both switches, but that change alone did not resolve Fa0/3's state.

## Verification

```text
show cdp neighbors
show etherchannel summary
show interfaces trunk
```

## Evidence still needed

1. One labeled topology screenshot showing the two inter-switch links.
2. A clear `show etherchannel summary` screenshot from **each** switch showing `Po1(SU)` and both ports `(P)`.
3. One screenshot of the actual mismatch error or pre-fix `Po1(SD)`/`(I)`/`(s)` output.
4. If demonstrating resilience, one screenshot showing a member down and Po1 still up, plus a successful PC-to-PC ping during that failure.

Save the final Packet Tracer `.pkt` file as well. Add text configs for the member ports on both switches before marking this lab complete.
