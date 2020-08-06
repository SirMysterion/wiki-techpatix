Snippet from HQ-EDGE:
```
crypto isakmp enable
crypto isakmp policy 10
authentication pre-share
encryption aes 256
hash sha
group 5
lifetime 3600
crypto isakmp key -REDACTED- address 172.16.78.100
crypto ipsec transform-set 50 esp-aes 256 esp-sha-hmac ah-sha-hmac
crypto ipsec security-association lifetime seconds 1800
crypto map MYMAP 10 ipsec-isakmp
match address 101
set peer 172.16.78.100
set pfs group5
set transform-set 50
set security-association lifetime seconds 900
access-list 101 permit gre any any
int gi0/3
crypto map MYMAP
```