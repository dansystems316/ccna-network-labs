# DHCP Snooping and DAI Trust Boundary Lab

**Status: Documentation ready.** First confirm that the chosen Packet Tracer switch model supports these commands; record unsupported features honestly.

## Ticket and topology

A user receives no DHCP lease after access security is enabled. Build a 2960 SW1 with VLAN 10: PC1 on Fa0/1 (untrusted), a legitimate DHCP server or router on G0/1 (trusted), and optionally a rogue server on Fa0/2 (untrusted) to show offer blocking. If testing DAI, use DHCP-acquired PC addresses so snooping has real IP/MAC/port bindings; a static device may need an explicit ARP ACL on platforms that support it. Configure one 192.168.10.0/24 DHCP pool with gateway 192.168.10.1.

On SW1, verify actual ports before applying:

```cisco
ip dhcp snooping
ip dhcp snooping vlan 10
interface GigabitEthernet0/1
 ip dhcp snooping trust
interface FastEthernet0/1
 ip dhcp snooping limit rate 15
ip arp inspection vlan 10
interface GigabitEthernet0/1
 ip arp inspection trust
```

## Verify and fault

1. With the legitimate server on G0/1, renew PC1 and capture `ipconfig /all`. Run `show ip dhcp snooping`, `show ip dhcp snooping binding`, `show ip arp inspection`, and `show ip arp inspection statistics`. Ping PC1's gateway to establish a working ARP path.
2. Inject a supported fault by removing `ip dhcp snooping trust` from G0/1. Force a new PC1 DHCP request; record whether the offer is dropped, the binding table, and PC1 result. Restore the trust command and retest DHCP and gateway ping. Treat DAI as a separate check; do not claim DAI caused a DHCP failure.
3. Optional: enable rogue DHCP service on Fa0/2. Observe whether its offer is blocked; capture the actual server ID and switch output. If Packet Tracer does not expose drop counters or cannot simulate the attack, keep the verified trusted-uplink case and say so.

## Finish gate

Add final `.pkt`, topology with trusted/untrusted ports, SW1/server configs, binding output, genuine failed and recovered client lease, ARP/gateway test, and fault record. Never fabricate a binding or drop counter. [Repository checklist](../PORTFOLIO_CHECKLIST.md).
