# IPv4

# IPv6
IPv6 is porvided via Hurricane Electric's [Tunnel Broker](https://tunnelbroker.net/) Service. The tunnel is terminated on the Edge router as a 6in4 tunnel. Each tunnel is provided a point to point /64 subnet address as well an assigned routed /48 network.

|          | Tunnel Endpoints     | Routed /64: (Not In use) | Routed /48:        |
| -------- |:--------------------:| :-----------------------:| :-----------------:|
| HQ Site  | 2001:470:7b:d7::2/64 | 2001:470:7c:d7::/64      | 2001:470:3857::/48 |
| BR Site  | 2001:470:7b:f8::2/64 | 2001:470:7c:f8::/64      | 2001:470:3858::/48 |

---

The following is the configurations for HQs IPv6 Tunnel
```
interface Tunnel6
 description Hurricane Electric IPv6 Tunnel Broker
 no ip address
 zone-member security OUTSIDE
 ipv6 address 2001:470:7B:D7::2/64
 ipv6 enable
 tunnel source 172.17.17.2
 tunnel mode ipv6ip
 tunnel destination 216.66.77.230
```


