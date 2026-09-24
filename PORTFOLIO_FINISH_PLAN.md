# Fast Portfolio Finish Plan

Complete the existing labs before starting a large capstone. This plan uses the repository's [completion checklist](PORTFOLIO_CHECKLIST.md): only mark a lab Complete when a saved Packet Tracer file, actual configs, observed tests, and failure/repair evidence exist.

## Best order

| Order | Lab | Smallest useful next task | Done when |
|---:|---|---|---|
| 1 | [VLAN routing](vlan-intervlan-routing/) | Export sanitized switch/router configs and name a PC1-to-PC2 ping capture | Trunk, gateways, VLAN fault and repair are traceable in the saved project |
| 2 | [OSPF](ospf/) | Export sanitized configs for all three routers | Neighbors, learned routes, PC ping, mask fault and repair match saved project |
| 3 | [STP](spanning-tree/) | Build triangle and capture root/alternate roles | Root change or uplink failure, reconvergence and repair proven |
| 4 | [EtherChannel](etherchannel/) | Build two-link LACP trunk | Both members bundled; one mismatch and fix documented |
| 5 | [ACL](acl/) | Write four-row policy matrix then build it | Browser permit/deny tests, placement, and repair proven |
| 6 | [NAT/PAT](nat-pat/) | Build PC–R1–R2–server path | Translation shown before and after inside-role fault |
| 7 | [DHCP Snooping/DAI](dhcp-snooping-dai/) | Confirm simulator supports commands | Binding, gateway/ARP result, trust fault and fix |
| 8 | [IPv6](ipv6-routing/) | Build two routed /64 LANs | Bidirectional routes, PC ping and return-route repair proven |

[DHCP relay](dhcp-relay/) is already Complete (owner verified). A strong first portfolio release is DHCP relay plus finished VLAN, OSPF, STP, and one access policy lab. The remaining playbooks can be completed afterward; none needs to be fabricated to make the existing portfolio useful. Do not start a giant capstone until these small cases are reproducible.

## Repeatable work session for each lab

1. Open its README and follow the addressing table; adapt interface numbers to your actual Packet Tracer devices.
2. Build and save the working project under `lab-name/packet-tracer/`.
3. Export `show running-config` from each device to `configs/`; remove secrets and real identifying details.
4. Capture the exact command outputs and labeled endpoint tests called for in the lab. Put only selected, readable images in `images/`.
5. Save a working baseline. Inject **one** fault, capture the client symptom and relevant device output, restore one setting, and rerun the same test.
6. Write `evidence/fault-case.md` using the [incident format](troubleshooting/README.md), with actual results and limitations. Reopen the saved `.pkt` and retest.
7. Change the index status to Complete only when the [required artifacts](PORTFOLIO_CHECKLIST.md) are present; otherwise leave it In progress.

Use `templates/lab-template.md` if a new lab needs a fresh README. One real, well-labeled fault case per lab is enough.
