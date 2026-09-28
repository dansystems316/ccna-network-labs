# EtherChannel / LACP troubleshooting

This is a separate Fa0/2–Fa0/3 troubleshooting exercise. The [completed Fa0/1–Fa0/2 failover lab](etherchannel-lacp/README.md) has its own Packet Tracer file and ping evidence.

## Goal

Bundle two links between Cisco 2960 switches using LACP. CDP identified Fa0/2 ↔ Fa0/2 and Fa0/3 ↔ Fa0/3. The trunk carries VLAN 10 with native VLAN 99.

## Fault and fix

The channel was initially down: `Po1(SD)` with standalone or suspended members. The switch reported incompatible DTP settings and allowed VLANs. One switch also lacked `switchport mode trunk` on its member ports.

Both ends were set to trunk mode with native VLAN 99, allowed VLAN 10, and LACP `active`. Fa0/2 then joined while Fa0/3 remained standalone or suspended. Removing and re-adding Fa0/3 to channel-group 1 on both switches produced this final observed result:

```text
1      Po1(SU)           LACP   Fa0/2(P) Fa0/3(P)
```

`SU` means the Layer 2 channel is in use; `P` means a physical port is bundled. VLAN 99 was also created on both switches, but that change alone did not resolve Fa0/3's state.

## VLAN mismatch: captured fault and repair

For a controlled fault, Fa0/3's allowed VLAN was changed from 10 to 20 on one switch. The switch reported `%EC-5-CANNOT_BUNDLE2` with `vlan mask is different`, and Fa0/3 became suspended while Fa0/2 kept Po1 in use. Restoring `switchport trunk allowed vlan 10` brought Fa0/3 back into the bundle.

| Evidence | What it shows |
|---|---|
| [VLAN mismatch and suspended member](images/vlan-mismatch-suspended.png) | The command, error, and `Po1(SU) Fa0/2(P) Fa0/3(s)` |
| [Switch1 repair](images/switch1-vlan-mismatch-fixed.png) | Restoring VLAN 10 and `Fa0/2(P) Fa0/3(P)` |
| [Switch0 working summary](images/switch0-both-members.png) | Both members bundled on Switch0 |

## Verification

```text
show cdp neighbors
show etherchannel summary
show interfaces trunk
```

## To finish this exercise

Add the final Packet Tracer `.pkt` file, a labeled topology image, and text configs for the member ports on both switches. The separate [failover lab](etherchannel-lacp/README.md) already documents a one-link failure and PC-to-PC pings; this exercise focuses on VLAN mismatch diagnosis.
