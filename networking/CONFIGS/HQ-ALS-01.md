```
!
! Last configuration change at 15:16:29 UTC Thu Jul 23 2020
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
!
hostname HQ-ALS-01
!
boot-start-marker
boot-end-marker
!
!
!
username ansible privilege 15 password 0  [REDACTED]
username thomas secret 5 [REDACTED]
username bradyn password 0  [REDACTED]
username rajesh password 0  [REDACTED]
username brennan password 0  [REDACTED]
username daniel password 0  [REDACTED]
username karanvir password 0  [REDACTED]
no aaa new-model
!
!
!
!
!
!
!
!
ip domain-list techpatix.com
no ip domain-lookup
ip domain-name techpatix.com
ip name-server 2600:70FF:B856:8210::10
ip name-server 2001:470:3857:8110::10
ip cef
ipv6 unicast-routing
ipv6 cef
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
vlan internal allocation policy ascending
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
!
interface GigabitEthernet0/0
 media-type rj45
 negotiation auto
!
interface GigabitEthernet0/1
 switchport access vlan 10
 switchport mode access
 media-type rj45
 negotiation auto
!
interface GigabitEthernet0/2
 switchport access vlan 10
 switchport mode access
 media-type rj45
 negotiation auto
!
interface GigabitEthernet0/3
 media-type rj45
 negotiation auto
!
interface GigabitEthernet1/0
 media-type rj45
 negotiation auto
!
interface GigabitEthernet1/1
 media-type rj45
 negotiation auto
!
interface GigabitEthernet1/2
 switchport trunk encapsulation dot1q
 switchport mode trunk
 media-type rj45
 negotiation auto
!
interface GigabitEthernet1/3
 switchport trunk encapsulation dot1q
 switchport mode trunk
 media-type rj45
 negotiation auto
!
interface GigabitEthernet2/0
 media-type rj45
 negotiation auto
!
interface GigabitEthernet2/1
 media-type rj45
 negotiation auto
!
interface GigabitEthernet2/2
 media-type rj45
 negotiation auto
!
interface GigabitEthernet2/3
 media-type rj45
 negotiation auto
!
interface GigabitEthernet3/0
 switchport access vlan 10
 media-type rj45
 negotiation auto
!
interface GigabitEthernet3/1
 media-type rj45
 negotiation auto
!
interface GigabitEthernet3/2
 media-type rj45
 negotiation auto
!
interface GigabitEthernet3/3
 media-type rj45
 negotiation auto
!
interface Vlan10
 no ip address
 shutdown
!
interface Vlan20
 no ip address
 shutdown
!
interface Vlan50
 ip address 172.16.150.3 255.255.255.0
 ipv6 address 2001:470:3857:8150::3/64
 ipv6 address 2600:70FF:B856:8150::3/64
!
ip forward-protocol nd
!
no ip http server
no ip http secure-server
!
ip route 0.0.0.0 0.0.0.0 172.16.150.254
ip ssh version 2
ip ssh pubkey-chain
  username thomas
   key-hash ssh-rsa  [REDACTED] 
!
!
ipv6 route ::/0 2001:470:3857:8150::1
!
!
snmp-server community techpatix RO
!
control-plane
!
line con 0
 logging synchronous
line aux 0
line vty 0 4
 login local
 transport input ssh
!
ntp server 142.11.210.231
!
end
```