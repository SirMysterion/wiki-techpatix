# IPv4
## DHCP

| Scope         | Range              | VLAN    | Site |
| ------------- |:------------------:|:-------:|:----:|
| 172.16.110.0  | 172.16.110.100-200 | VLAN 10 | HQ   |
| 172.16.120.0  | 172.16.120.100-200 | VLAN 20 | HQ   |
| 172.16.210.0  | 172.16.210.100-200 | VLAN 10 | BR   |
| 172.16.220.0  | 172.16.220.100-200 | VLAN 20 | BR   |


## Address assignment
| Hosts                     | IPv4          | VLAN     |
| ------------------------- |:-------------:| :-------:|
| HQ-EDGE.techpatix.com     | 172.16.100.1  | N/A      |
| HQ-CORE-01.techpatix.com  | 172.16.150.1  | VLAN 50  |
| HQ-CORE-02.techpatix.com  | 172.16.150.2  | VLAN 50  |
| HQ-ALS-01.techpatix.com   | 172.16.150.3  | VLAN 50  |
| BR-EDGE.techpatix.com     | 172.16.200.1  | N/A      |
| BR-CORE-01.techpatix.com  | 172.16.250.1  | VLAN 50  |
| BR-ALS-01.techpatix.com   | 172.16.250.3  | VLAN 50  |
| HQ-DC-01.techpatix.com    | 172.16.110.10 | VLAN 10  |
| HQ-FS-01.techpatix.com    | 172.16.110.15 | VLAN 10  |
| BR-DC-01.techpatix.com    | 172.16.210.10 | VLAN 10  |
| BR-FS-01.techpatix.com    | 172.16.210.15 | VLAN 10  |

## EtherChannel
The following is the configuration for the routed EtherChannel link between Core HQ Switches:
### Core-01
```
interface G3/2-3/3
channel-group 1 mode on
```
```
interface port-channel 1 
no switchport
ip add 172.16.100.9 255.255.255.252
```

### Core-02
```
interface G3/2-3/3
channel-group 1 mode on
```
```
interface port-channel 1 
no switchport
ip add 172.16.100.10 255.255.255.252
```


---



# IPv6
IPv6 is provided via Hurricane Electric's [Tunnel Broker](https://tunnelbroker.net/) Service. The tunnel is terminated on the Edge router as a 6in4 tunnel. Each tunnel is provided a point to point /64 subnet address as well an assigned routed /48 network.

|          | Tunnel Endpoints     | Routed /64: (Not In use) | Routed /48:        |
| -------- |:--------------------:| :-----------------------:| :-----------------:|
| HQ Site  | 2001:470:7b:d7::2/64 | 2001:470:7c:d7::/64      | 2001:470:3857::/48 |
| BR Site  | 2001:470:7b:f8::2/64 | 2001:470:7c:f8::/64      | 2001:470:3858::/48 |


| Hosts                     | IPv6                   | VLAN     |
| ------------------------- |:----------------------:| :-------:|
| HQ-EDGE.techpatix.com     | 2001:470:3857:810A::A  | N/A      |
| HQ-CORE-01.techpatix.com  | 2001:470:3857:8150::1  | VLAN 50  |
| HQ-CORE-02.techpatix.com  | 2001:470:3857:8150::2  | VLAN 50  |
| HQ-ALS-01.techpatix.com   | 2001:470:3857:8150::3  | VLAN 50  |
| BR-EDGE.techpatix.com     | 2001:470:3858:820A::A  | N/A      |
| BR-CORE-01.techpatix.com  | 2001:470:3858:8250::1  | VLAN 50  |
| BR-ALS-01.techpatix.com   | 2001:470:3858:8250::3  | VLAN 50  |
| HQ-DC-01.techpatix.com    | 2001:470:3857:8110::10 | VLAN 10  |
| HQ-FS-01.techpatix.com    | 2001:470:3857:8110::15 | VLAN 10  |
| BR-DC-01.techpatix.com    | 2001:470:3858:8210::10 | VLAN 10  |
| BR-FS-01.techpatix.com    | 2001:470:3858:8210::15 | VLAN 10  |



---

The following is the configuration for HQs IPv6 Tunnel:
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



---




