```
!
! Last configuration change at 03:33:29 UTC Wed Aug 5 2020
! NVRAM config last updated at 03:33:30 UTC Wed Aug 5 2020
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
!
hostname BR-SW-01
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
ip dhcp excluded-address 172.16.210.0 172.16.210.49
ip dhcp excluded-address 172.16.210.201 172.16.210.255
!
ip dhcp pool vlan10
 network 172.16.210.0 255.255.255.0
 default-router 172.16.210.254 
 dns-server 172.16.210.10 
 domain-name techpatix.com
!
!
ip domain-list techpatix.com
no ip domain-lookup
ip domain-name techpatix.com
ip name-server 2001:470:3857:8110::10
ip name-server 2001:470:3858:8210::10
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
 switchport trunk encapsulation dot1q
 switchport mode trunk
 media-type rj45
 negotiation auto
 ipv6 eigrp 1
!
interface GigabitEthernet0/1
 no switchport
 ip address 172.16.200.2 255.255.255.252
 ip summary-address eigrp 1 172.16.192.0 255.255.192.0
 negotiation auto
 ipv6 address 2001:470:3858:820A::B/64
 ipv6 eigrp 1
!
interface GigabitEthernet0/2
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
 media-type rj45
 negotiation auto
!
interface GigabitEthernet1/3
 media-type rj45
 negotiation auto
!
interface Vlan10
 ip address 172.16.210.1 255.255.255.0
 ipv6 address FE80::1 link-local
 ipv6 address 2001:470:3858:8210::1/64
 ipv6 eigrp 1
!
interface Vlan20
 ip address 172.16.220.1 255.255.255.0
 ip helper-address 172.16.210.10 
 ipv6 address FE80::1 link-local
 ipv6 address 2001:470:3858:8220::1/64
 ipv6 eigrp 1
!
interface Vlan50
 ip address 172.16.250.1 255.255.255.0
 ipv6 address FE80::1 link-local
 ipv6 address 2001:470:3858:8250::1/64
 ipv6 eigrp 1
!
!
router eigrp 1
 network 172.16.200.0 0.0.0.3
 network 172.16.210.0 0.0.0.255
 network 172.16.220.0 0.0.0.255
 network 172.16.250.0 0.0.0.255
!
ip forward-protocol nd
!
no ip http server
no ip http secure-server
!
ip route 0.0.0.0 0.0.0.0 172.16.200.1
ip ssh version 2
ip ssh pubkey-chain
  username thomas
   key-hash ssh-rsa [REDACTED] 
!
!
ipv6 router eigrp 1
 eigrp router-id 2.2.1.1
!
!
!
snmp-server community techpatix RO
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
ntp server 142.11.210.231 minpoll 10
!
end
```
