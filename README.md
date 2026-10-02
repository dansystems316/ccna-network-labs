# Dan Partain | Cisco Networking Portfolio

Hands-on Cisco Packet Tracer projects for entry-level IT support, network support, and NOC roles. These labs document device configuration, command-line verification, and troubleshooting with before-and-after evidence. CCNA preparation is in progress.

## Start with these projects

| Project | What to inspect | Evidence status |
|---|---|---|
| [DHCP relay troubleshooting](dhcp-relay/README.md) | Isolate a failed lease to a missing VLAN 10 helper; use VLAN 20 as a working control | Complete evidence set; owner verified the saved project |
| [OSPF adjacency troubleshooting](ospf/README.md) | Diagnose a subnet-mask mismatch; verify neighbors, learned routes, and end-to-end reachability | Complete evidence set; configs reviewed, no independent simulation during this review |
| [ACL rule-order troubleshooting](acl-troubleshooting/README.md) | Show how an early permit bypasses a deny, then restore policy and inspect hit counts | Project and failure/repair evidence included; full router export pending |

## Additional hands-on labs

| Project | Demonstrated work | Remaining evidence |
|---|---|---|
| [VLAN and inter-VLAN routing](vlan-intervlan-routing/README.md) | Access VLAN fault isolation, router-on-a-stick, gateway recovery | Full configs, final trunk forwarding and reconciled endpoint/ping evidence |
| [STP failover](spanning-tree/README.md) | Root election, blocked backup path, forwarding after link failure | Three switch config exports and link-restoration capture |
| [LACP failover](etherchannel/etherchannel-lacp/README.md) | Bundled links and connectivity with one member down | Two switch exports and labeled topology image |
| [EtherChannel VLAN mismatch](etherchannel/README.md) | Suspended member caused by inconsistent allowed VLANs; repair evidence | This separate exercise's saved project and configs |
| [DHCP snooping and DAI](dhcp-snooping-dai/README.md) | VLAN 10 snooping, two learned bindings, trust boundaries, DAI active | Invalid ARP rejection and rogue DHCP blocking remain unverified |

[Full lab index](LAB_INDEX.md) · [Troubleshooting cases](troubleshooting/README.md) · [Evidence checklist](PORTFOLIO_CHECKLIST.md) · [Remaining tasks](PORTFOLIO_NEXT_STEPS.md)

## How I troubleshoot

Define the symptom and expected behavior, establish the affected scope, check interface and VLAN state, inspect addressing and routes, make one controlled change, and repeat the original test. Each documented result distinguishes observed behavior from tests still needed.

## Using the labs

Open the linked README first for addressing, requirements, and evidence. Download its `.pkt` file and open it in Cisco Packet Tracer. Configuration exports are included where available; screenshots retain the observed device state. A complete evidence set is not a claim of independent execution by the documentation reviewer.

## Planned work

[NAT/PAT](nat-pat/README.md) and [IPv6 routing](ipv6-routing/README.md) are outlines awaiting lab artifacts. They are not presented as demonstrated skills. Finish the existing evidence gaps before expanding the portfolio.

## About

Built by Dan Partain while preparing for the CCNA and moving into IT. The portfolio focuses on practical troubleshooting, accurate documentation, and verification for support and networking work.
