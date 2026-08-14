# IPv6 Routing Labs

## Planned Skills

- Configure global unicast IPv6 addresses
- Understand link-local addresses
- Enable IPv6 routing
- Configure IPv6 static, default, host, and floating static routes
- Verify IPv6 neighbor discovery and routing

## Basic Configuration

```text
ipv6 unicast-routing

interface gigabitethernet0/0
 ipv6 address 2001:db8:10::1/64
 no shutdown
```

## Static Route Examples

```text
ipv6 route 2001:db8:20::/64 2001:db8:12::2
ipv6 route ::/0 2001:db8:12::2
ipv6 route 2001:db8:30::10/128 2001:db8:12::2
```

## Verification Commands

```text
show ipv6 interface brief
show ipv6 route
show ipv6 neighbors
ping ipv6 <address>
traceroute ipv6 <address>
```

## Troubleshooting Focus

Check prefix length, next-hop reachability, interface state, and whether `ipv6 unicast-routing` is enabled before changing route configuration.
