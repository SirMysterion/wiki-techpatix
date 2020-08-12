```
!
! Last configuration change at 11:31:13 UTC Sat Aug 8 2020
!
version 15.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname HQ-EDGE
!
boot-start-marker
boot-end-marker
!
!
!
no aaa new-model
!
!
!
mmi polling-interval 60
no mmi auto-configure
no mmi pvc
mmi snmp-timeout 180
!
!
!
!
!
!
!
!
!
!
!
!
!
ip domain list techpatix.com
ip domain name techpatix.com
ip name-server 2001:470:3857:8110::10
ip name-server 2001:470:3858:8210::10
ip cef
ipv6 unicast-routing
ipv6 cef
!
multilink bundle-name authenticated
!
!
cts logging verbose
!
!
username ansible privilege 15 password 0  [REDACTED]
username thomas secret 5 [REDACTED]
username bradyn password 0  [REDACTED]
username rajesh password 0  [REDACTED]
username brennan password 0  [REDACTED]
username daniel password 0  [REDACTED]
username karanvir password 0  [REDACTED]
!
redundancy
!
!
ip ssh version 2
ip ssh pubkey-chain
  username thomas
   key-hash ssh-rsa [REDACTED] 
!
class-map type inspect match-any Proto41-CLASS
 match access-group 105
class-map type inspect match-any INSIDE-TO-OUTSIDE-CLASS
 match access-group name INSIDE-TO-OUTSIDE
 match access-group name INSIDE-TO-OUTSIDE_v6
class-map type inspect match-any OUTSIDE-TO-INSIDE-CLASS
 match access-group name OUTSIDE-TO-INSIDE
 match access-group name OUTSIDE-TO-INSIDE_v6
 match protocol dns
class-map type inspect match-any SELF-TO-OUTSIDE-CLASS
 match access-group name SELF-TO-OUTSIDE
 match access-group name SELF-TO-OUTSIDE_v6
class-map type inspect match-any OUTSIDE-TO-SELF-CLASS
 match access-group name OUTSIDE-TO-SELF
 match access-group name OUTSIDE-TO-SELF_v6
class-map type inspect match-any INSIDE-TO-SELF-CLASS
 match access-group name SELF-AND-INSIDE
 match access-group name SELF-AND-INSIDE_v6
!
policy-map type inspect SELF-TO-OUTSIDE-POLICY
 class type inspect Proto41-CLASS
  pass
 class type inspect SELF-TO-OUTSIDE-CLASS
  inspect 
 class class-default
  drop log
policy-map type inspect OUTSIDE-TO-SELF-POLICY
 class type inspect Proto41-CLASS
  pass
 class type inspect OUTSIDE-TO-SELF-CLASS
  inspect 
 class class-default
  drop log
policy-map type inspect INSIDE-TO-SELF-POLICY
 class type inspect INSIDE-TO-SELF-CLASS
  pass
 class class-default
  drop log
policy-map type inspect SELF-TO-INSIDE-POLICY
 class type inspect INSIDE-TO-SELF-CLASS
  pass
 class class-default
  drop log
policy-map type inspect INSIDE-TO-OUTSIDE-POLICY
 class type inspect INSIDE-TO-OUTSIDE-CLASS
  inspect 
 class class-default
  drop log
policy-map type inspect OUTSIDE-TO-INSIDE-POLICY
 class type inspect OUTSIDE-TO-INSIDE-CLASS
  inspect 
 class class-default
  drop log
!
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
! 
!
!
!
!
crypto isakmp policy 10
 encr aes 256
 authentication pre-share
 group 5
 lifetime 3600
crypto isakmp key sMpw8hXqELa9gEG7qWYQ address 172.16.78.100  
!
crypto ipsec security-association lifetime seconds 1800
!
crypto ipsec transform-set 50 ah-sha-hmac esp-aes 256 esp-sha-hmac 
 mode tunnel
!
!
!
crypto map MYMAP 10 ipsec-isakmp 
 set peer 172.16.78.100
 set security-association lifetime seconds 900
 set transform-set 50 
 set pfs group5
 match address 101
!
!
!
!
!
interface Tunnel0
 ip address 172.16.30.1 255.255.255.252
 zone-member security INSIDE
 ipv6 address 2001:470:3857:81FF::1/64
 tunnel source GigabitEthernet0/3
 tunnel destination 172.16.78.100
!
interface Tunnel6
 description Hurricane Electric IPv6 Tunnel Broker
 no ip address
 zone-member security OUTSIDE
 ipv6 address 2001:470:7B:D7::2/64
 ipv6 enable
 tunnel source 172.17.17.2
 tunnel mode ipv6ip
 tunnel destination 216.66.77.230
!
interface GigabitEthernet0/0
 no ip address
 ip virtual-reassembly in
 zone-member security OUTSIDE
 duplex auto
 speed auto
 media-type rj45
 ipv6 address autoconfig
!
interface GigabitEthernet0/1
 ip address 172.16.100.1 255.255.255.252
 ip nat inside
 ip virtual-reassembly in
 zone-member security INSIDE
 duplex full
 speed auto
 media-type rj45
 ipv6 address 2001:470:3857:810A::A/64
!
interface GigabitEthernet0/2
 ip address 172.16.100.5 255.255.255.252
 ip nat inside
 ip virtual-reassembly in
 zone-member security INSIDE
 duplex full
 speed auto
 media-type rj45
 ipv6 address 2001:470:3857:810B::A/64
!
interface GigabitEthernet0/3
 ip address 172.16.74.100 255.255.255.0
 zone-member security INSIDE
 duplex auto
 speed auto
 media-type rj45
 ipv6 address autoconfig
 crypto map MYMAP
!
interface GigabitEthernet0/4
 no ip address
 shutdown
 duplex auto
 speed auto
 media-type rj45
!
interface GigabitEthernet0/5
 no ip address
 shutdown
 duplex auto
 speed auto
 media-type rj45
!
interface GigabitEthernet0/6
 ip address 172.17.17.2 255.255.255.252
 ip nat outside
 ip virtual-reassembly in
 zone-member security OUTSIDE
 duplex auto
 speed auto
 media-type rj45
!
!
router eigrp techpatix
 !
 address-family ipv6 unicast autonomous-system 1
  !
  af-interface GigabitEthernet0/0
   passive-interface
  exit-af-interface
  !
  af-interface GigabitEthernet0/6
   passive-interface
  exit-af-interface
  !
  af-interface Tunnel0
   summary-address 2600:70FF:B856:8100::/56
   summary-address 2001:470:3857::/48
  exit-af-interface
  !
  topology base
   distribute-list prefix-list HQ-LAN out Tunnel0
   redistribute static metric 10000 0 255 1 1500
  exit-af-topology
  eigrp router-id 1.1.0.1
 exit-address-family
 !
 address-family ipv4 unicast autonomous-system 1
  !
  topology base
  exit-af-topology
  network 172.16.30.0 0.0.0.3
  network 172.16.100.0 0.0.0.3
  network 172.16.100.4 0.0.0.3
  eigrp router-id 1.1.0.1
 exit-address-family
!
ip forward-protocol nd
!
!
no ip http server
no ip http secure-server
ip nat inside source list 100 interface GigabitEthernet0/6 overload
ip nat inside source static tcp 172.16.110.15 80 interface GigabitEthernet0/6 80
ip nat inside source static tcp 172.16.110.15 443 interface GigabitEthernet0/6 443
ip nat inside source static tcp 172.16.110.15 21 interface GigabitEthernet0/6 21
ip route 0.0.0.0 0.0.0.0 172.17.17.1
ip route 172.16.78.0 255.255.255.0 172.16.74.1
!
ip access-list extended INSIDE-TO-OUTSIDE
 permit tcp any any
 permit udp any any
 permit icmp any any
ip access-list extended OUTSIDE-TO-INSIDE
 permit icmp any any
 permit udp any eq domain any
 permit udp any any eq ntp
 permit tcp any host 172.16.110.15 eq www
 permit tcp any host 172.16.110.15 eq 443
 permit tcp any host 172.16.110.15 eq ftp
 deny   ip any any
ip access-list extended OUTSIDE-TO-SELF
 permit 41 any any
 permit icmp any any
 permit tcp any any eq 22
 permit udp any any eq netbios-ns
 permit udp any any eq netbios-dgm
 permit eigrp any any
 permit ipinip any any
 deny   ip any any
ip access-list extended SELF-AND-INSIDE
 permit ip any any
 deny   ip any any
ip access-list extended SELF-TO-OUTSIDE
 permit 41 any any
 permit tcp any any
 permit udp any any
 permit eigrp any any
 permit icmp any any
!
!
ip prefix-list HQ-LANv4 seq 5 permit 172.16.100.0/24
ip prefix-list HQ-LANv4 seq 10 permit 172.16.110.0/24
ip prefix-list HQ-LANv4 seq 15 permit 172.16.120.0/24
ip prefix-list HQ-LANv4 seq 20 permit 172.16.150.0/24
ipv6 route 2600:70FF:B856:8200::/56 Tunnel0
ipv6 route ::/0 Tunnel6
!
!
ipv6 prefix-list DefaultRouteV6 seq 5 deny ::/0
!
ipv6 prefix-list HQ-LAN seq 5 permit 2001:470:3857::/48
ipv6 prefix-list HQ-LAN seq 10 permit 2600:70FF:B856:8100::/56
snmp-server community techpatix RO
!
access-list 10 permit 172.16.120.0 0.0.0.255
access-list 10 permit 172.16.150.0 0.0.0.255
access-list 10 permit 172.16.110.0 0.0.0.255
access-list 10 permit 172.16.100.0 0.0.0.255
access-list 100 deny   41 any any
access-list 100 deny   tcp host 172.16.110.15 eq www any
access-list 100 permit ip 172.16.100.0 0.0.0.255 any
access-list 100 permit ip 172.16.110.0 0.0.0.255 any
access-list 100 permit ip 172.16.120.0 0.0.0.255 any
access-list 100 permit ip 172.16.150.0 0.0.0.255 any
access-list 101 permit gre any any
access-list 105 permit ip any host 216.66.77.230
access-list 105 permit ip host 216.66.77.230 any
access-list 105 deny   ip any any
!
ipv6 access-list INSIDE-TO-OUTSIDE_v6
 permit tcp any any
 permit udp any any
 permit icmp any any
!
ipv6 access-list OUTSIDE-TO-INSIDE_v6
 permit icmp any any
 permit ipv6 2600:70FF:B856::/48 any
 permit udp any any eq snmp
 permit udp any any eq snmptrap
 permit ipv6 2600:70FF:B856:8000::/52 any
 permit tcp 2600:70FF:B856::/48 any eq 3389
 permit udp 2600:70FF:B856::/48 any eq 3389
 permit tcp any any eq 10050
 permit tcp any any eq 10051
 permit tcp any host 2001:470:3857:8110::10 eq 10050
 permit tcp any host 2001:470:3857:8110::10 eq 10051
 permit tcp any host 2001:470:3857:8110::15 eq 10050
 permit tcp any host 2001:470:3857:8110::15 eq 10051
 permit udp any host 2600:70FF:B856:8110::10 eq domain
 permit tcp any any eq 22
 permit tcp any host 2001:470:3857:8110::10 eq 3389
 permit udp any host 2001:470:3857:8110::10 eq 3389
 permit tcp any host 2001:470:3857:8110::15 eq 3389
 permit udp any host 2001:470:3857:8110::15 eq 3389
 permit tcp any host 2001:470:3857:8110::15 eq ftp
 permit tcp any host 2001:470:3857:8110::15 eq www
 permit tcp any host 2001:470:3857:8110::15 eq 443
 sequence 4294967294 deny ipv6 any any
!
ipv6 access-list OUTSIDE-TO-SELF_v6
 permit icmp any any
 permit tcp any any eq 22
 permit udp any any eq snmptrap
 permit udp any any eq snmp
 sequence 4294967294 deny ipv6 any any
!
ipv6 access-list SELF-AND-INSIDE_v6
 permit ipv6 any any
 sequence 4294967294 deny ipv6 any any
!
ipv6 access-list SELF-TO-OUTSIDE_v6
 permit tcp any any
 permit udp any any
 permit icmp any any
!
control-plane
!
!
line con 0
 logging synchronous
line aux 0
line vty 0 4
 login local
 transport input ssh
!
no scheduler allocate
ntp server 142.11.210.231 minpoll 10
!
end
```
