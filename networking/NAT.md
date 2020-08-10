# PAT

Both HQ and Branch sites were configured with Port Address Translation on Edge Routers as follows:
## HQ
```
interface G0/1-G0/2
ip nat inside
interface G0/6
ip nat outside
```
```
ip nat insdie source list 100 interface G0/6 overload
```
```
access-list 100 deny tcp host 172.16.110.15 eq www any
access-list 100 permit ip 172.16.100.0 0.0.0.255 any
access-list 100 permit ip 172.16.110.0 0.0.0.255 any
access-list 100 permit ip 172.16.120.0 0.0.0.255 any
access-list 100 permit ip 172.16.150.0 0.0.0.255 any
```


## BR
```
interface G0/1
ip nat inside
interface G0/6
ip nat outiside
```
```
ip nat inside source list 100 interface G0/6 overload
```
```
access-list 100 permit ip 172.16.200.0 0.0.0.255 any
access-list 100 permit ip 172.16.210.0 0.0.0.255 any
access-list 100 permit ip 172.16.220.0 0.0.0.255 any
access-list 100 permit ip 172.16.250.0 0.0.0.255 any
```

---

# Port Forwarding

HQ site Edge Router was configured with port forwarding to allow external addresses to reach the internal file server:

```
ip nat inside source static tcp 172.16.110.15 80 interface G0/6 80
ip nat inside source static tcp 172.16.110.15 443 interface G0/6 443
```
```
ip access-list extended OUTSIDE-TO-INSIDE
permit tcp any host 172.16.110.15 eq www
permit tcp any host 172.16.110.15 eq 443
permit tcp any host 172.16.110.15 eq ftp
deny ip any any
```
