# EtherChannel / LACP troubleshooting

## Scenario

Two Cisco 2960 switches in Packet Tracer use two physical uplinks as one Layer 2 LACP EtherChannel. CDP confirmed Fa0/2 ↔ Fa0/2 and Fa0/3 ↔ Fa0/3. The trunk carries VLAN 10 and uses native VLAN 99. The goal is for both links to join Port-channel1 and preserve connectivity when one member fails.

**Status:** Configuration and troubleshooting documented. Upload the final Packet Tracer file, topology, both switch configurations, screenshots, and a tested link-failure result before marking the lab complete.

## Verification target

On **each** switch:

```text
show etherchannel summary

1      Po1(SU)           LACP   Fa0/2(P) Fa0/3(P)
```

`SU` means a Layer 2 port-channel is in use. `P` means the physical interface is bundled. `I` is a standalone interface; `s` is suspended. The final `Po1(SU) Fa0/2(P) Fa0/3(P)` output was observed on one switch in the troubleshooting session. Capture the peer's matching output for the repository evidence.

## Troubleshooting record

| Observation | Evidence | Action and result |
|---|---|---|
| Channel initially failed to form | `Po1(SD)`; `Fa0/2(I)` and `Fa0/3(s)` | Used `show cdp neighbors` to confirm the two cable pairs before changing member ports. |
| Local members did not match | `%EC-5-CANNOT_BUNDLE2` reported a DTP mode difference and later a VLAN mask difference | Made both member ports trunks with the same native VLAN 99 and allowed VLAN 10. |
| Peer member mode differed | One switch had `switchport mode trunk` on Fa0/2–3; the other did not | Configured trunk mode on the peer's Fa0/2–3. Port-channel1 came up and Fa0/2 joined, while Fa0/3 remained outside the bundle. |
| Second member still not bundled | `Po1(SU) Fa0/2(P) Fa0/3(I)` on one switch and `Fa0/3(s)` on the other | Removed and re-added Fa0/3's `channel-group 1 mode active` membership on both switches; the final observed summary showed both members `(P)`. |

VLAN 99 initially displayed as `Inactive`, so it was created on both switches. That change alone did **not** resolve Fa0/3's membership; avoid assigning it the sole root cause. The session exposed more than one configuration mismatch, and the exact reason the final membership reset was needed was not isolated further.

## Final member settings

The two switch-to-switch members were configured consistently on both ends:

```text
interface range fastethernet0/2-3
 switchport trunk native vlan 99
 switchport trunk allowed vlan 10
 switchport mode trunk
 channel-group 1 mode active
```

VLAN 99 was created on both switches. Fa0/1 was a separate port and was not part of this EtherChannel.

## Evidence to add

- Final `.pkt` file and labeled topology screenshot.
- `show running-config` sections for Po1 and Fa0/2–3 on **both** switches.
- `show etherchannel summary` on **both** switches showing `Po1(SU)` and two `(P)` members.
- A normal PC-to-PC ping and, if tested again in this final file, a one-link failure ping and EtherChannel summary. Label screenshots by the test they actually show.

Useful Packet Tracer commands: `show cdp neighbors`, `show etherchannel summary`, `show interfaces trunk`, `show interfaces fa0/3 switchport`, and `show running-config`. This Packet Tracer switch rejected `show lacp neighbor`; the summary output was used for LACP membership evidence.
