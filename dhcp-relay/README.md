# DHCP Relay Troubleshooting Lab

> **Status: Documentation ready.** This is a build and evidence plan. No Packet Tracer file, device exports, topology image, or real verification output has been added yet. Do not call this lab complete until the repository's [evidence checklist](../PORTFOLIO_CHECKLIST.md) is satisfied.

## Support Ticket and Objective

PC1 on VLAN 10 cannot obtain an IPv4 lease from a DHCP server on a different routed network. Build a two-client, two-VLAN network with R1 as the gateway and DHCP relay, and R2 as the central DHCP server. Establish a working baseline, remove the helper from one client gateway to reproduce the ticket, locate the fault, restore the helper, then prove both clients receive the correct lease and can reach the server. Record actual observations only after building the lab.

## Planned Topology

```text
PC1 (VLAN 10) -- SW1 -- 802.1Q trunk -- R1 -- 10.0.12.0/30 -- R2 (DHCP server)
PC2 (VLAN 20) -- SW1 -- same trunk -- R1
```

Save a labeled Packet Tracer topology screenshot to `images/topology.png` once built. Interface names below assume routers with GigabitEthernet0/0 and GigabitEthernet0/1; adjust the names to the actual Packet Tracer model and record that choice.

| Device | Interface / VLAN | Address or expected lease | Gateway / purpose |
|---|---|---|---|
| PC1 | NIC / VLAN 10 | DHCP: `192.168.10.100–199/24` | `192.168.10.1` |
| PC2 | NIC / VLAN 20 | DHCP: `192.168.20.100–199/24` | `192.168.20.1` |
| SW1 | PC1 access port | VLAN 10 | Client attachment |
| SW1 | PC2 access port | VLAN 20 | Client attachment |
| SW1 | R1 uplink | Trunk allowing 10, 20 | Router on a stick |
| R1 | G0/0.10 | `192.168.10.1/24` | VLAN 10 gateway and helper |
| R1 | G0/0.20 | `192.168.20.1/24` | VLAN 20 gateway and helper |
| R1 | G0/1 | `10.0.12.1/30` | Routed link to R2 |
| R2 | G0/0 | `10.0.12.2/30` | DHCP service address |

R2 needs return routes to `192.168.10.0/24` and `192.168.20.0/24` through `10.0.12.1`. Set R2 DHCP pools `VLAN10` and `VLAN20` to the corresponding `/24` networks, default routers `.1`, and exclude `.1–.99` and `.200–.254` from assignment. No DNS service is required for this ticket. PC addresses in the table are expected ranges, not observed leases.

## Key Configuration to Build

Example IOS commands, to be adapted to the actual interface names. Save full sanitized running configurations separately after testing.

```cisco
! SW1: assign PC access ports to VLAN 10 and VLAN 20;
! make the R1 uplink a trunk allowing 10,20.
! R1
interface GigabitEthernet0/0
 no shutdown
interface GigabitEthernet0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
 ip helper-address 10.0.12.2
interface GigabitEthernet0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
 ip helper-address 10.0.12.2
interface GigabitEthernet0/1
 ip address 10.0.12.1 255.255.255.252
 no shutdown
```

```cisco
! R2
interface GigabitEthernet0/0
 ip address 10.0.12.2 255.255.255.252
 no shutdown
ip route 192.168.10.0 255.255.255.0 10.0.12.1
ip route 192.168.20.0 255.255.255.0 10.0.12.1
ip dhcp excluded-address 192.168.10.1 192.168.10.99
ip dhcp excluded-address 192.168.10.200 192.168.10.254
ip dhcp excluded-address 192.168.20.1 192.168.20.99
ip dhcp excluded-address 192.168.20.200 192.168.20.254
ip dhcp pool VLAN10
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
ip dhcp pool VLAN20
 network 192.168.20.0 255.255.255.0
 default-router 192.168.20.1
```

## Verification and Evidence Plan

Run the exact commands supported by the chosen Packet Tracer IOS image. Save copied text in `evidence/` with headings identifying the device, test stage, and command; use screenshots only as supporting evidence. On PCs, use Desktop > IP Configuration to request or renew DHCP, and Command Prompt for the commands below. Packet Tracer's PC CLI may not support every desktop Windows `ipconfig` switch; toggling Static then DHCP in IP Configuration can force a fresh request.

| Stage | Device | Exact command or test | Expected observation |
|---|---|---|---|
| Baseline link/VLAN | SW1 | `show vlan brief` | PC access ports in the intended VLANs |
| Baseline trunk | SW1 | `show interfaces trunk` | VLANs 10 and 20 permitted and active |
| Gateway state | R1 | `show ip interface brief` | Both subinterfaces and server link up/up |
| Helper placement | R1 | `show running-config interface GigabitEthernet0/0.10` and `show running-config interface GigabitEthernet0/0.20` | `ip helper-address 10.0.12.2` under both client-facing subinterfaces |
| Server reachability | R1 | `ping 10.0.12.2` | Replies from R2 |
| Return path | R2 | `show ip route 192.168.10.0` and `show ip route 192.168.20.0` | Static routes via `10.0.12.1` |
| Server pool setup | R2 | `show running-config | section ip dhcp` | Pools, exclusions, and default routers match plan (if pipe syntax is unavailable, use `show running-config`) |
| Server lease state | R2 | `show ip dhcp binding` and `show ip dhcp pool` | Leases and pool usage reflect client requests |
| Client lease | PC1, PC2 | `ipconfig /all` | Correct subnet, mask, and gateway per VLAN; note actual assigned addresses |
| Client reachability | PC1, PC2 | `ping 192.168.10.1` or `ping 192.168.20.1` as applicable; `ping 10.0.12.2` | Gateway and DHCP server reachable after assignment |

Some Packet Tracer router images omit `show ip dhcp pool` or filtered running-config forms. Note the unsupported command and capture the equivalent full running-config and binding output; never claim an unrun check passed.

## Fault Injection and Troubleshooting Record

First save proof of the working baseline. Then remove only `ip helper-address 10.0.12.2` from **R1 G0/0.10** with `no ip helper-address 10.0.12.2`. Renew PC1 and capture the failed DHCP request in `evidence/pc1-before.txt` or `images/pc1-before.png`; keep PC2 working as a scope control. An old lease can hide the fault, so force a new request and note how it was done. The exact failure message or fallback address must come from the lab.

Follow the [repository troubleshooting workflow](../troubleshooting/README.md): define the symptom and expected state, scope the fault to PC1/VLAN 10, test from client/access port toward gateway/server, compare command outputs, form a hypothesis, make one change, and repeat the original test. Record the following in `evidence/fault-case.md` after the actual experiment:

1. Ticket, affected client, time of test, and expected lease.
2. Before: PC1 lease screen/output, PC2 control lease, SW1 VLAN/trunk state, R1 subinterface state and helper lines, R1-to-R2 ping, R2 pool/bindings and return route.
3. Hypothesis and observation identifying the **confirmed** missing helper on R1 VLAN 10. Do not attribute every DHCP failure to relay: verify VLAN, gateway, server reachability, pool, and return path.
4. Exact corrective change under R1 G0/0.10: `ip helper-address 10.0.12.2`.
5. After: repeat the same PC1 request; capture `ipconfig /all`, PC1 gateway/server pings, R2 `show ip dhcp binding`, and R1 helper configuration. Record the actual leased IP, mask, and gateway. Confirm PC2 still works.
6. Prevention: compare helpers across client gateway interfaces and preserve a known-good addressing plan before a change.

## Folder Structure and Completion Gate

```text
dhcp-relay/
├── README.md
├── packet-tracer/
│   └── dhcp-relay.pkt                 # add after building and testing
├── configs/
│   ├── R1-running-config.txt         # add after export
│   ├── R2-running-config.txt
│   └── SW1-running-config.txt
├── evidence/
│   ├── baseline.txt                  # actual IOS/PC output
│   ├── pc1-before.txt
│   ├── pc1-after.txt
│   └── fault-case.md
└── images/
    ├── topology.png
    ├── pc1-before.png                # optional supporting screenshots
    └── pc1-after.png
```

Directories remain empty until actual artifacts exist; filenames above are targets, not links or completed evidence. A complete lab needs the real topology, addressing plan, `.pkt`, sanitized device configurations, actual verification output, and a documented failure with before/after proof and root cause. Update its status to **In progress** when genuine artifacts arrive and **Complete** only when all criteria in [the portfolio checklist](../PORTFOLIO_CHECKLIST.md) are met.
