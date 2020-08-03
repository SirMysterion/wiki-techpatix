Cisco Zone Based Firewall (ZBFW) is made up in a way that you define rules and ACLs between zones. Once those zones are configured you can assign interfaces/vlans to be apart of zones and have the rules apply in that way.

HQ and BR Site have very similar ZBFW rules setup with only two Zones. Zone INSIDE and Zone OUTSIDE. The zone "self" is the default cisco include zone that applies to traffic originating or destined for the router it's self

Once the zones are defined there also needs to be zone-pairs defined such as "source OUTSIDE destination INSIDE"

The following is the zones being defined and paired
```
zone security INSIDE
zone security OUTSIDE
zone-pair security IN-TO-OUT source INSIDE destination OUTSIDE
 service-policy type inspect INSIDE-TO-OUTSIDE-POLICY
zone-pair security OUT-TO-IN source OUTSIDE destination INSIDE
 service-policy type inspect OUTSIDE-TO-INSIDE-POLICY
zone-pair security SELF-TO-IN source self destination INSIDE
 service-policy type inspect SELF-TO-INSIDE-POLICY
zone-pair security IN-TO-SELF source INSIDE destination self
 service-policy type inspect INSIDE-TO-SELF-POLICY
zone-pair security SELF-TO-OUT source self destination OUTSIDE
 service-policy type inspect SELF-TO-OUTSIDE-POLICY
zone-pair security OUT-TO-SELF source OUTSIDE destination self
 service-policy type inspect OUTSIDE-TO-SELF-POLICY
```

Each Zone-Pair refers to a policy such as the following. (only OUTSIDE-TO-SELF-POLICY shown )
This policy includes the most configuration as exceptions were needed to allow the IPv6 Tunnel to be connected and terminated
at the router it's self. In this case. all "Proto41-CLASS" is simply allowed to and from the Tunnel Endpoint. The inspect clause on the outside to self class allows traffic inbound if traffic matches OUTSIDE-TO-SELF-CLASS or if it is related to existing connections. The default action is to drop traffic and log.
```
policy-map type inspect OUTSIDE-TO-SELF-POLICY
 class type inspect Proto41-CLASS
  pass
 class type inspect OUTSIDE-TO-SELF-CLASS
  inspect 
 class class-default
  drop log
```

Here is where the class can match both IPv4 and IPv6 by allowing traffic that matches any of the Access-Lists. One for IPv4 and one for IPv6.
```
class-map type inspect match-any OUTSIDE-TO-SELF-CLASS
 match access-group name OUTSIDE-TO-SELF
 match access-group name OUTSIDE-TO-SELF_v6
```

The following is the Access-Lists that allows traffic from WAN into LAN (NOTE See PortForwarding Documentation for v4)
```
ip access-list extended OUTSIDE-TO-INSIDE
 permit icmp any any
 permit udp any eq domain any
 permit udp any any eq ntp
 permit tcp any host 172.16.110.15 eq www
 deny   ip any any
```
```
ipv6 access-list OUTSIDE-TO-INSIDE_v6
 permit icmp any any
 permit ipv6 2600:70FF:B856::/48 any
 permit udp any any eq snmp
 permit udp any any eq snmptrap
 permit tcp any any eq 22
 permit tcp any any eq 10050
 permit tcp any any eq 10051
 permit ipv6 2600:70FF:B856:8000::/52 any
 permit tcp 2600:70FF:B856::/48 any eq 3389
 permit udp 2600:70FF:B856::/48 any eq 3389
 permit tcp any host 2001:470:3857:8110::10 eq 3389
 permit udp any host 2001:470:3857:8110::10 eq 3389
 permit tcp any host 2001:470:3857:8110::15 eq 3389
 permit udp any host 2001:470:3857:8110::15 eq 3389
 sequence 4294967294 deny ipv6 any any
```
