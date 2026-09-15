# DHCP Snooping and Dynamic ARP Inspection Labs

## Planned Skills

- Enable DHCP Snooping globally and per VLAN
- Mark legitimate DHCP-facing ports as trusted
- Rate-limit DHCP messages on access ports
- Use the DHCP Snooping binding database
- Enable Dynamic ARP Inspection
- Troubleshoot dropped DHCP and ARP traffic

## Configuration Example

```text
ip dhcp snooping
ip dhcp snooping vlan 10,20

interface gigabitethernet0/1
 ip dhcp snooping trust
 ip arp inspection trust

interface range fastethernet0/1-20
 ip dhcp snooping limit rate 15

ip arp inspection vlan 10,20
```

## Verification Commands

```text
show ip dhcp snooping
show ip dhcp snooping binding
show ip arp inspection
show ip arp inspection statistics
```

## Troubleshooting Focus

A trusted-port mistake can block legitimate DHCP offers or ARP traffic. Verify which interface leads toward the legitimate DHCP server or upstream switch before changing trust state.
