# NAT-PAT
<h2>Descriptio</h2><br>
<p>This project demonstrates the implementation of <b>Network Address Translation (NAT)</b>, <b>Port Access Translation (PAT)</b><br>
and a <b>Static NAT</b> in a multi-router network environment using Cisco router devices.
  
<b>Static NAT</b> is configured in <b>CO-1</b> to provide one-to-one mappings for internal host, While <b>Dynamic NAT</b> was applied to <br>
<b>CO-2</b> (many private IP to many public IP address) and <b>CO-3</b> (many private IP to one public IP address) translating private IP address <br>
using a multiple public IP address pool. PAT was used to allow multiple private IP to share a public IP address through port numberings.

VLANs and Router-on-a-Stick are used on ISP side to separate the three company networks while sharing the same physical interface. For connectivity EIGRP was implement with autonomous system # 199. </p> 
