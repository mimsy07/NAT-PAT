# NAT-PAT
<h2>Description</h2><br>
<p>This project demonstrates the implementation of <b>Network Address Translation (NAT)</b>, <b>Port Access Translation (PAT)</b> and a <b>Static NAT</b> in a multi-router network environment using Cisco router devices.
  
<b>Static NAT</b> is configured in <b>CO-1</b> to provide one-to-one mappings for internal host, while <b>Dynamic NAT</b> was applied to <b>CO-2</b> (many private IP to many public IP address) and <b>CO-3</b> (many private IP to one public IP address) translating private IP address using a multiple public IP address pool. PAT was used to allow multiple private IP to share a public IP address through port numberings.

VLANs and Router-on-a-Stick are used on ISP side to separate the three company networks while sharing the same physical interface. For connectivity EIGRP was implement with autonomous system # 199. </p> 

<h3>Main Objectives</h3>

 1. Allow company to pass through internet by translating private IPs to public IP addresses.
 2. Configure EIGRP routing protocol to provide routing and connectivity between the different networks.
 3. Implement VLANs to logically separate the company-1, company-2 and company-3.


<h3>Key Skills Demonstrated</h3>

-  CISCO IOS NAT/PAT configuration
-  Implementation of Dynamic NAT, Static NAT
-  Configuring VLANs and assigning ports
-  Configuring IEE 802.1Q encapsulation to sub-interfaces
-  Testing connectivity
-  Assigning designated IP addresses to the interface
-  Implementing EIGRP routing protocol
    

<h2>Project Walk through</h2>

<p align="center">
Network Diagram: <br/>
<img src="https://github.com/mimsy07/NAT-PAT/blob/main/NAT%20topo.png" height="80%" width="80%"/>
<br />

<p align="center">
Company's IP addresses and port assigned: <br/>
<img src="https://github.com/mimsy07/NAT-PAT/blob/main/ip%20address.png" height="50%" width="50%"/>
<br/>


<p align="center">
ISP: <br/>
<img src="https://github.com/mimsy07/NAT-PAT/blob/main/ISP.png" height="50%" width="50%"/>
<br />

<h2>Configuration</h2>

<h3>COMPANY-1</h3>

Static NAT
```
ip nat inside source static 192.168.10.1 20.30.10.10 
ip nat inside source static 192.168.10.2 20.30.10.20 
ip nat inside source static 192.168.10.3 20.30.10.30 
ip nat inside source static 192.168.10.100 20.30.10.40 
!
int g0/1
ip nat inside
exit
!
int g0/0
ip nat outside
exit

```

EIGRP
```
router eigrp 199
network 192.168.10.0 255.255.255.0
network 20.30.10.0 255.255.255.0
no auto-summary
```


<h3>COMPANY-2</h3>

Dynamic NAT
```
ip nat pool cmpny-2 25.35.42.3 25.35.42.7 netmask 255.255.255.224 
access-list 20 permit 172.16.10.0 0.0.0.255 
ip nat inside source list 20 pool cmpny-2 overload    
!
int g0/1
ip nat inside
exit
!
int
int g0/0
ip nat outside 
```

EIGRP
```
router eigrp 199
network 172.16.10.0 255.255.255.0
network 25.35.42.0 255.255.255.224
no auto-summary
```


<h3>COMPANY-3</h3>

Dynamic NAT
```
ip nat pool cmpny-3 130.30.30.1 130.30.30.1 netmask 255.255.255.252
access-list 30 permit 10.10.10.0 0.0.0.255 
ip nat inside source list 30 pool cmpny-3 overload    
!
int g0/1
ip nat inside
exit
!
int
int g0/0
ip nat outside 
```

EIGRP
```
router eigrp 199
network 10.10.10.0 255.255.255.0
network 130.30.30.0 255.255.255.252
no auto-summary
```

<h3>ISP ROUTER</h3>

<p>Initializing g0/1 interface, configuring sub-interfaces and implementing trunking encapsulation</p>

```
int g0/1
no ip address
no shut
!
int g0/1.110
encapsulation dot1q 110
ip address 20.30.10.2 255.255.255.0
exit
!
int g0/1.210
encapsulation dot1q 210
ip address 25.35.42.2 255.255.255.224
exit
!
int g0/1.310
encapsulation dot1q 310
ip address 130.30.30.2 255.255.255.252
exit

```

EIGRP

<p>Advertise directly connected network including sub-interface network</p>

```
router eigrp 199
network 200.10.20.0 255.255.255.0
network 20.30.10.0 255.255.255.0
network 25.35.42.0 255.255.255.224
network 130.30.30.0 255.255.255.252
no auto-summary
```

<h3>ISP SW1</h3>

<p>Assigning switchport mode, creating VLANs and assigning the ports </p>

```
vlan 110
name CO-1
exit
!
vlan 210
name CO-2
exit
vlan 310
name CO-3
exit

int 5/1
switchport mode trunk
exit
!
int range 6/1, 7/1, 8/1
switchport mode access
exit
!
int 6/1
switchport access vlan 110
exit
!
int 7/1
switchport access vlan 210
exit
!
int 8/1
switchport access vlan 310
exit
!
```

<h3>Others</h3>

WEB-1
<p>Assigning IP address and implement a default static route (since they are stub router)</p>

```
int g0/0
ip address 200.10.20.10 255.255.255.0
no shut
exit
!
ip route 0.0.0.0 0.0.0.0 g0/0

```

WEB-2
<p>Assigning IP address and implement a default static route (WEB-2 are also a stub router)</p>

```
int g0/0
ip address 200.10.20.20 255.255.255.0
no shut
exit
!
ip route 0.0.0.0 0.0.0.0 g0/0

```

SERVER-1
<p>Assigning IP address and implement a default static route</p>

```
int g0/0
ip address 200.10.20.30 255.255.255.0
no shut
exit
!
ip route 0.0.0.0 0.0.0.0 g0/0

```
