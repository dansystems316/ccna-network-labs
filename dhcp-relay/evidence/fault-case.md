# DHCP Relay Fault Case — Evidence in Progress

## Ticket and Expected State

A client on VLAN 10 should receive a `192.168.10.0/24` DHCP lease from R2 (`10.0.12.2`) through R1 G0/0.10 and use gateway `192.168.10.1`. The earlier [client lease](../images/pc1-lease.png), [R1 helper configuration](../images/r1-helper-config.png), and [server bindings](../images/r2-dhcp-bindings.png) show a working baseline before the submitted fault screenshots. Exact capture times and whether the later saved `.pkt` includes this experiment has not been checked by opening it.

## Reported Change and Observed Failure

| Step | Submitted evidence | Direct observation | Limit |
|---|---|---|---|
| Change | [VLAN 10 helper removal](../images/fault-remove-vlan10-helper.png) | `R1(config)#int g0/0.10` followed by `R1(config-subif)#no ip helper-address 10.0.12.2` | Command and target interface are visible; a separate earlier crop shows the same removal command without interface context. |
| Fault scope | [PC1 failed, PC2 working](../images/fault-pc1-failed-pc2-working.png) | PC1 title shows `DHCP request failed` with no IPv4 address displayed; PC2 title shows `DHCP request successful`, `192.168.20.21/24`, gateway `192.168.20.1` | The screenshot has both clients side by side. It does not itself show the router command or the method used to force the request. |
| Failed test | [APIPA result](../images/fault-client-apipa.png) | DHCP failed; FastEthernet0 has `169.254.87.151/16`, gateway `0.0.0.0` | The crop does not show the PC title or how a fresh request was forced. |
| Repair | [Restore helper](../images/repair-restore-helper.png) | `R1(config-subif)#ip helper-address 10.0.12.2` | The crop does not identify the subinterface. |
| Retest | [Restored lease](../images/repair-client-lease.png) | DHCP request successful; `192.168.10.21/24`, gateway `192.168.10.1`, DNS `1.1.1.1` | The crop does not show the PC title or post-repair pings. |
| Identified PC1 lease | [PC1 title and successful request](../images/repair-pc1-identified-lease.png) | PC1 window shows DHCP successful with `192.168.10.21/24`, gateway `192.168.10.1` | This identifies a successful PC1 lease, but its timing relative to the cropped failure cannot be proved from the images alone. |

## Root Cause and Scope

The captured change confirms that R1 G0/0.10's helper was removed. PC1 failed to get a lease while PC2 on VLAN 20 continued to receive one. The VLAN 10 DHCP broadcasts could not reach the remote server without the relay on the client-facing subinterface. The separate APIPA crop records one fallback result but lacks a PC title; use the side-by-side image for client attribution. After the helper was restored, an identified PC1 screenshot shows a valid lease. The submitted images establish the fault, scope, and recovery sequence, though the exact request method and capture times are not recorded.

## Confirmation Still Needed

1. The supplied [R1](../configs/R1-running-config.txt), [R2](../configs/R2-running-config.txt), and [SW1](../configs/SW1-running-config.txt) configs show the final helper, pool, and VLAN settings. The [later saved `.pkt`](../dhcp-relay-final.pkt) is uploaded; open it in Packet Tracer to confirm it contains these settings.
2. The [identified PC1 server ping](../images/pc1-to-server-ping-identified.png) and [identified PC2 server ping](../images/pc2-to-server-ping.png) each show their title and 4/4 replies. Their exact timing relative to the helper restoration is not visible; the earlier [PC1 ping crop](../images/pc1-to-server-ping-unattributed.png) lacked its title. Capture a current R2 `show ip dhcp binding` if a contemporaneous post-repair binding is needed. The existing binding and gateway ping captures have no proved timing relative to this repair.
3. Record how the fresh PC1 request was forced and the exact test order if known. Do not invent those details from screenshots.

## Prevention and Faster Check

Compare helper addresses on all client-facing gateway interfaces, verify the remote pool and return routes, and force a new DHCP request before declaring a fix. Use a second VLAN client as a scope control when only one client segment reports address failures.
