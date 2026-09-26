# LACP EtherChannel with link failover

## Objective

Build a two-link Layer 2 EtherChannel between Switch0 and Switch1 in Cisco Packet Tracer. Verify both links join Port-channel1, test host connectivity in VLAN 10, then show that connectivity survives one member link going down.

## Topology

- Two switches joined by two FastEthernet links in LACP Port-channel1.
- Port-channel1 is a Layer 2 trunk allowing VLAN 10.
- PC0: `192.168.10.12`; PC1: `192.168.10.15`.
- [Open the Packet Tracer lab](etherchannel-lacp.pkt) for the full topology and saved device configuration.

## Verification with both links up

On each switch, `show etherchannel summary` displayed `Po1(SU)` with `Fa0/1(P)` and `Fa0/2(P)`. `S` means Layer 2, `U` means in use, and `P` means bundled in the port channel.

| Check | Observed result | Evidence |
| --- | --- | --- |
| Switch0 | `Po1(SU)`, `Fa0/1(P)`, `Fa0/2(P)` | [Switch0 summary](switch0-both-links.png) |
| Switch1 | `Po1(SU)`, `Fa0/1(P)`, `Fa0/2(P)` | [Switch1 summary](switch1-both-links.png) |
| PC0 → PC1 | Four replies from `192.168.10.15`, 0% loss | [PC0 ping](pc0-ping-normal.png) |
| PC1 → PC0 | Four replies from `192.168.10.12`, 0% loss | [PC1 ping](pc1-ping-normal.png) |

## Single-link failure test

I shut down Fa0/1 on Switch0 and checked `show etherchannel summary` on both switches. Port-channel1 stayed in use (`SU`), Fa0/1 showed `D` (down), and Fa0/2 stayed bundled (`P`). While that member was down, each PC received four out of four ping replies from the other. These screenshots show connectivity over the remaining member after the shutdown; they do not measure convergence time or packet loss during the transition.

| Check | Observed result | Evidence |
| --- | --- | --- |
| Switch0 | `Po1(SU)`, `Fa0/1(D)`, `Fa0/2(P)` | [Switch0 failover](switch0-one-link-down.png) |
| Switch1 | `Po1(SU)`, `Fa0/1(D)`, `Fa0/2(P)` | [Switch1 failover](switch1-one-link-down.png) |
| PC0 → PC1 | Four replies, 0% loss | [PC0 failover ping](pc0-ping-failover.png) |
| PC1 → PC0 | Four replies, 0% loss | [PC1 failover ping](pc1-ping-failover.png) |

## Troubleshooting

Earlier in the build, `show etherchannel summary` showed one member as `I` (stand-alone), although `Po1(SU)` was up through the other member. I compared both ends and corrected the lab until both intended ports showed `P` at the same time. This taught me to verify member status, rather than relying only on the port channel being up. During the intentional failure test, `D` indicated the shut-down member; Po1 stayed up through Fa0/2.

## Reproduce

1. Open the `.pkt` file and check `show etherchannel summary` on both switches.
2. Ping `192.168.10.15` from PC0 and `192.168.10.12` from PC1.
3. On Switch0, enter `configure terminal`, `interface fa0/1`, `shutdown`, `end`, and `show etherchannel summary`.
4. Repeat the pings with Fa0/1 down.
5. Restore it with `configure terminal`, `interface fa0/1`, `no shutdown`, `end`. Verify both member ports return to `P` on both switches.
