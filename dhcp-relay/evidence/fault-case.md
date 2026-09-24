# DHCP Relay Fault Case — Evidence in Progress

## Ticket and Expected State

A client on VLAN 10 should receive a `192.168.10.0/24` DHCP lease from R2 (`10.0.12.2`) through R1 G0/0.10 and use gateway `192.168.10.1`. The earlier [client lease](../images/pc1-lease.png), [R1 helper configuration](../images/r1-helper-config.png), and [server bindings](../images/r2-dhcp-bindings.png) show a working baseline before the submitted fault screenshots. Exact capture times and whether the saved `.pkt` includes this later experiment are not established.

## Reported Change and Observed Failure

| Step | Submitted evidence | Direct observation | Limit |
|---|---|---|---|
| Change | [Remove helper](../images/fault-remove-helper.png) | `R1(config-subif)#no ip helper-address 10.0.12.2` | The crop does not identify the subinterface. |
| Failed test | [APIPA result](../images/fault-client-apipa.png) | DHCP failed; FastEthernet0 has `169.254.87.151/16`, gateway `0.0.0.0` | The crop does not show the PC title or how a fresh request was forced. |
| Repair | [Restore helper](../images/repair-restore-helper.png) | `R1(config-subif)#ip helper-address 10.0.12.2` | The crop does not identify the subinterface. |
| Retest | [Restored lease](../images/repair-client-lease.png) | DHCP request successful; `192.168.10.21/24`, gateway `192.168.10.1`, DNS `1.1.1.1` | The crop does not show the PC title or post-repair pings. |

## Working Hypothesis

Removing the helper from R1's VLAN 10 client gateway would prevent a broadcast DHCP request from reaching R2. The command, failure, restored command, and successful lease are consistent with that explanation. The submitted crops alone do not conclusively establish that G0/0.10 was the interface changed or that PC2 continued working during the fault. Keep this as a supported hypothesis until the device/interface and scope are captured in the same incident record.

## Confirmation Still Needed

1. Capture `show running-config interface GigabitEthernet0/0.10` during the fault and after repair, including a visible Router0 identity. Preserve G0/0.20 helper state as the control.
2. Capture PC1's title and a forced fresh DHCP request during the fault, plus PC2's successful lease at the same stage. Note whether DHCP was renewed by toggling Static then DHCP or another method.
3. Capture post-repair PC1 `ipconfig /all`, gateway and server pings, and R2 `show ip dhcp binding`; export final sanitized configs for all three devices and save the repaired `.pkt`.
4. Add the confirmed root cause, exact test order, and prevention note here after checking those observations. Never relabel the initial working baseline as post-fix proof.
