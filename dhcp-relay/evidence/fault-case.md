# DHCP Relay Fault Record

| Troubleshooting step | Observation and evidence |
|---|---|
| Symptom and expected state | PC1 on VLAN 10 should receive a `192.168.10.0/24` lease from R2 `10.0.12.2` through R1 G0/0.10. PC2 on VLAN 20 is the control. |
| Baseline checks | [SW1 VLANs](../images/sw1-vlans.png), [trunk](../images/sw1-trunk.png), [R1 interfaces](../images/r1-interfaces.png), [R2 routes](../images/r2-return-routes-installed.png), [pools](../configs/R2-running-config.txt), and [bindings](../images/r2-dhcp-bindings.png). These captures were not one simultaneous snapshot. |
| Hypothesis and test | [R1 CLI](../images/fault-remove-vlan10-helper.png) shows `interface g0/0.10` followed by `no ip helper-address 10.0.12.2`. |
| Observed failure and scope | [PC1 fails to request DHCP while PC2 receives `192.168.20.21/24`](../images/fault-pc1-failed-pc2-working.png). Both clients are named in the image. |
| Root cause | Without the helper on R1 G0/0.10, PC1's broadcast cannot reach R2. VLAN 20 retains its helper. |
| Corrective change | [R1 CLI](../images/repair-restore-helper.png) shows `ip helper-address 10.0.12.2`; this crop omits the interface, while the [final R1 config](../configs/R1-running-config.txt) shows helpers on both subinterfaces. |
| Post-fix result | [PC1 lease](../images/repair-pc1-identified-lease.png) shows `192.168.10.21/24` with gateway `192.168.10.1`. [PC1](../images/pc1-to-server-ping-identified.png) and [PC2](../images/pc2-to-server-ping.png) each ping R2 4/4. The owner reports reopening the [final `.pkt`](../packet-tracer/dhcp-relay.pkt) and confirming both leases and pings. |
| Prevention | Compare gateway helpers, verify the VLAN/trunk, pool and return route, and force a new DHCP request while keeping the unaffected VLAN as a control. |

**Evidence limits:** Screenshot times and the method used to force the failed request were not recorded. The restoration screenshot crops its interface. Packet Tracer was unavailable in this workspace; the reopened-project check is owner reported.
