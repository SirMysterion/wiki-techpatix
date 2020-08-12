```
!
! Last configuration change at 23:43:41 UTC Tue Aug 4 2020 by ansible
! NVRAM config last updated at 23:54:22 UTC Tue Aug 4 2020 by ansible
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
!
hostname BR-ALS-01
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
ip domain-name techpatix.com
ip name-server 2001:470:3857:8110::10
ip name-server 2001:470:3858:8210::10
ip cef
no ipv6 cef
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
 media-type rj45
 negotiation auto
!
interface GigabitEthernet1/3
 media-type rj45
 negotiation auto
!
interface Vlan50
 ip address 172.16.250.3 255.255.255.0
 ipv6 address 2001:470:3858:8250::3/64
!
ip forward-protocol nd
!
no ip http server
no ip http secure-server
!
ip route 0.0.0.0 0.0.0.0 172.16.250.1
ip ssh version 2
ip ssh pubkey-chain
  username thomas
   key-hash ssh-rsa [REDACTED] 
!
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
