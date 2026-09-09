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
<img src="https://github.com/mimsy07/NAT-PAT/blob/main/Untitled.png" height="80%" width="80%"/>
<br />
