# DHCP Relay Fault Case — Evidence in Progress

## Ticket and Expected State

A client on VLAN 10 should receive a `192.168.10.0/24` DHCP lease from R2 (`10.0.12.2`) through R1 G0/0.10 and use gateway `192.168.10.1`. The earlier [client lease](../images/pc1-lease.png), [R1 helper configuration](../images/r1-helper-config.png), and [server bindings](../images/r2-dhcp-bindings.png) show a working baseline before the submitted fault screenshots. Exact capture times and whether the saved `.pkt` includes this later experiment are not established.

## Reported Change and Observed Failure

| Step | Submitted evidence | Direct observation | Limit |
|---|---|---|---|
| Change | [VLAN 10 helper removal](../images/fault-remove-vlan10-helper.png) | `R1(config)#int g0/0.10` followed by `R1(config-subif)#no ip helper-address 10.0.12.2` | Command and target interface are visible; a separate earlier crop shows the same removal command without interface context. |
| Failed test | [APIPA result](../images/fault-client-apipa.png) | DHCP failed; FastEthernet0 has `169.254.87.151/16`, gateway `0.0.0.0` | The crop does not show the PC title or how a fresh request was forced. |
| Repair | [Restore helper](../images/repair-restore-helper.png) | `R1(config-subif)#ip helper-address 10.0.12.2` | The crop does not identify the subinterface. |
| Retest | [Restored lease](../images/repair-client-lease.png) | DHCP request successful; `192.168.10.21/24`, gateway `192.168.10.1`, DNS `1.1.1.1` | The crop does not show the PC title or post-repair pings. |
| Identified PC1 lease | [PC1 title and successful request](../images/repair-pc1-identified-lease.png) | PC1 window shows DHCP successful with `192.168.10.21/24`, gateway `192.168.10.1` | This identifies a successful PC1 lease, but its timing relative to the cropped failure cannot be proved from the images alone. |

## Working Hypothesis

The captured change confirms that R1 G0/0.10's helper was removed. Without a relay, a new DHCP broadcast on VLAN 10 cannot reach R2 across the routed link. The submitted APIPA result and restored lease are consistent with that cause. The failed-client image still lacks a PC title, and PC2's state during the fault is absent, so the exact observed scope remains qualified.

## Confirmation Still Needed

1. Capture `show running-config interface GigabitEthernet0/0.10` during the fault and after repair, including a visible Router0 identity. Preserve G0/0.20 helper state as the control.
2. Capture PC1's title and a forced fresh DHCP request during the fault, plus PC2's successful lease at the same stage. Note whether DHCP was renewed by toggling Static then DHCP or another method.
3. Capture post-repair PC1 gateway and server pings with its title visible, and a current R2 `show ip dhcp binding`; export final sanitized configs for all three devices and save the repaired `.pkt`.
4. Add the confirmed root cause, exact test order, and prevention note here after checking those observations. Never relabel the initial working baseline as post-fix proof.
