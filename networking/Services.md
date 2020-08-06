## IPv4
DHCP is provided by HQ-DC-01 and BR-DC-01 on VLAN10  
IP-Helper address is used on HQ-CORE-01, HQ-CORE-02 and BR-SW-01 for VLAN 20 Traffic

DNS is provided by HQ-DC-01 and BR-DC-01
## IPv6
SLAAC provides IPv6 addressing for all VLAN
DNS is provided by HQ-DC-01 and BR-DC-01


## Web Services
HQ-FS-01 is hosting an IIS Service  
Port 80 = HTTP to HTTPS Re-write rule   
Port 433 = internal.techpatix.com as well as www.techpatix.com (split DNS)
