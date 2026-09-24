# Rapid PVST+ Root and Failover Lab

**Status: Documentation ready.** Build and test in Packet Tracer; no lab artifacts are claimed.

## Ticket and design

After an uplink fails, users briefly lose connectivity. Determine the VLAN 10 root bridge, the alternate port, and whether the network converges when the active path drops. Use three 2960 switches in a triangle: SW1–SW2, SW2–SW3, SW3–SW1. Put one PC in VLAN 10 on SW2 and another on SW3. Give both PCs static addresses in 192.168.10.0/24 so they can ping across the switched topology; no gateway is needed for same-subnet testing. Use a separate router or L3 gateway only if testing beyond VLAN 10.

| Item | Example |
|---|---|
| PC-A / PC-B | 192.168.10.11/24 / 192.168.10.12/24 |
| Root | SW1 for VLAN 10 |
| SW2, SW3 uplinks | Trunks carrying VLAN 10 |
| Access ports | VLAN 10, PortFast only on PC ports |

Set `spanning-tree mode rapid-pvst` on all switches. On SW1 use `spanning-tree vlan 10 root primary`. Configure matching trunks and VLAN 10 on every switch. Never enable PortFast on switch-to-switch uplinks.

## Verify and inject one fault

1. Record PC-A to PC-B ping, `show vlan brief`, `show interfaces trunk`, `show spanning-tree vlan 10`, and `show spanning-tree root` on the switches. Write down SW1 root ID, each root port, and the alternate/discarding port. Do not assume a particular interface wins a tie; use the output.
2. Shut the currently forwarding SW1 uplink at one end. Repeat `show spanning-tree vlan 10` on SW2 and SW3, and ping PC-B again. Note which alternate becomes forwarding and whether reachability returns. Restore the link with `no shutdown`.
3. For the troubleshooting case, deliberately set SW2 as VLAN 10 root with `spanning-tree vlan 10 root primary` or lower SW2's priority, then capture the changed root ID. Restore SW1's intended lower priority and capture the repaired root roles. Record actual priorities; the `root primary` macro may choose a different value than expected.

## Finish gate

Add `packet-tracer/spanning-tree.pkt`, a topology screenshot, sanitized SW1/SW2/SW3 configs, before/fault/after root and port-role outputs, PC ping results, and `evidence/fault-case.md` with symptom, hypothesis, change, result, and prevention. Reopen the saved project and retest before marking Complete. [Repository checklist](../PORTFOLIO_CHECKLIST.md).
