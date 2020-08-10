# PAT
---

Both HQ and Branch sites were configured with Port Address Translation as follows:
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

