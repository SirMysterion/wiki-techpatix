```
!
! Last configuration change at 15:16:35 UTC Thu Jul 23 2020
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
!
hostname HQ-CORE-02
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
ip dhcp excluded-address 172.16.110.0 172.16.110.49
ip dhcp excluded-address 172.16.110.201 172.16.110.255
!
ip dhcp pool vlan10
 network 172.16.110.0 255.255.255.0
 default-router 172.16.110.254 
 dns-server 172.16.110.10 
 domain-name techpatix.com
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
track 1 interface GigabitEthernet0/1 line-protocol
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
interface Port-channel1
 no switchport
 ip address 172.16.100.10 255.255.255.252
!
interface GigabitEthernet0/0
 media-type rj45
 negotiation auto
!
interface GigabitEthernet0/1
 no switchport
 ip address 172.16.100.6 255.255.255.252
 ip summary-address eigrp 1 172.16.0.0 255.255.0.0
 negotiation auto
 ipv6 address 2001:470:3857:810B::2/64
 ipv6 address 2600:70FF:B856:810B::2/64
 ipv6 eigrp 1
!
interface GigabitEthernet0/2
 switchport trunk encapsulation dot1q
 switchport mode trunk
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
 media-type rj45
 negotiation auto
!
interface GigabitEthernet3/1
 media-type rj45
 negotiation auto
!
interface GigabitEthernet3/2
 no switchport
 no ip address
 negotiation auto
 channel-group 1 mode on
!
interface GigabitEthernet3/3
 no switchport
 no ip address
 negotiation auto
 channel-group 1 mode on
!
interface Vlan10
 ip address 172.16.110.2 255.255.255.0
 standby version 2
 standby 1 ipv6 FE80::1
 standby 1 priority 95
 standby 1 preempt
 standby 1 track 1 decrement 10
 standby 2 ip 172.16.110.254
 standby 2 priority 95
 standby 2 preempt
 standby 2 track 1 decrement 10
 ipv6 address 2001:470:3857:8110::2/64
 ipv6 eigrp 1
!
interface Vlan20
 ip address 172.16.120.2 255.255.255.0
 standby version 2
 standby 1 ipv6 FE80::1
 standby 1 preempt
 standby 1 track 1 decrement 10
 standby 2 ip 172.16.120.254
 standby 2 preempt
 standby 2 track 1 decrement 10
 ipv6 address 2001:470:3857:8120::2/64
 ipv6 address 2600:70FF:B856:8120::2/64
 ipv6 eigrp 1
!
interface Vlan50
 ip address 172.16.150.2 255.255.255.0
 standby version 2
 standby 1 ipv6 FE80::1
 standby 1 preempt
 standby 1 track 1 decrement 10
 standby 2 ip 172.16.150.254
 standby 2 preempt
 standby 2 track 1 decrement 10
 ipv6 address 2001:470:3857:8150::2/64
 ipv6 address 2600:70FF:B856:8150::2/64
 ipv6 enable
 ipv6 eigrp 1
!
!
router eigrp 1
 network 172.16.100.0 0.0.0.255
 network 172.16.110.0 0.0.0.255
 network 172.16.120.0 0.0.0.255
 network 172.16.150.0 0.0.0.255
 eigrp router-id 1.1.2.2
!
ip forward-protocol nd
!
no ip http server
no ip http secure-server
!
ip route 0.0.0.0 0.0.0.0 172.16.100.5
ip ssh version 2
ip ssh pubkey-chain
  username thomas
   key-hash ssh-rsa [REDACTED] 
!
!
ipv6 router eigrp 1
 eigrp router-id 1.1.2.2
!
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
ntp server 142.11.210.231 minpoll 10
!
end
```