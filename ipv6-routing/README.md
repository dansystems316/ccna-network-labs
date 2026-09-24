# IPv6 Static Route Troubleshooting Lab

**Status: Documentation ready.** Addresses are a plan until verified in Packet Tracer.

## Ticket and topology

PC1 can ping its local IPv6 gateway but not a remote PC. Build PC1—R1—R2—PC2. Use documentation prefixes:

| Link | R1 / R2 / PC address |
|---|---|
| LAN 1 `2001:db8:10::/64` | R1 `2001:db8:10::1`; PC1 `2001:db8:10::10` |
| Transit `2001:db8:12::/64` | R1 `2001:db8:12::1`; R2 `2001:db8:12::2` |
| LAN 2 `2001:db8:20::/64` | R2 `2001:db8:20::1`; PC2 `2001:db8:20::20` |

Enable `ipv6 unicast-routing` on both routers and assign IPv6 addresses to the corresponding enabled interfaces. Set PC gateways to the local router IPv6 address or let Router Advertisements supply them, then record which method you used. Add R1 `ipv6 route 2001:db8:20::/64 2001:db8:12::2` and R2 `ipv6 route 2001:db8:10::/64 2001:db8:12::1`.

## Verify and fault

1. Capture `show ipv6 interface brief`, `show ipv6 route`, and `show ipv6 neighbors` on both routers. Ping each router's transit neighbor. From PC1 ping local gateway, then PC2 `2001:db8:20::20`; traceroute if supported by the PC.
2. Remove R2's return route with `no ipv6 route 2001:db8:10::/64 2001:db8:12::1`. Repeat PC1-to-PC2 test; capture R2 route table and contrast it with directly connected routes. Restore the route, ping again, and record the route entry and result. Verify a failed test actually occurs before stating the root cause; existing routes can hide the fault.
3. Explain that neighbor discovery validates local next-hop reachability while the static routes supply the remote prefix and return path.

## Finish gate

Add final `.pkt`, topology, R1/R2 configs, both routing tables, neighbor outputs, identified PC pings before/fault/after, and root-cause record. Reopen saved project. [Repository checklist](../PORTFOLIO_CHECKLIST.md).
