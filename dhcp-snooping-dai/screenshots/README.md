# Screenshot evidence

- 01-topology.png: PC1 on Fa0/1, PC2 on Fa0/2, Router0 on Gi0/1, server on Fa0/3.
- 02-dhcp-snooping.png: enabled for VLAN 10, option 82 disabled, Fa0/1 untrusted and Gi0/1 trusted. Fa0/2 is not visible in this image.
- 03-dhcp-bindings.png: .22 on Fa0/1 and .21 on Fa0/2, both VLAN 10, total bindings 2.
- 04-dai-status.png: DAI Enabled / Active on VLAN 10, counters zero.
- 05-connectivity.png: four successful ping replies from .21; source IP is not visible.

Screenshots verify configuration, bindings and a connectivity result. DAI rejection and rogue DHCP blocking remain unverified.
