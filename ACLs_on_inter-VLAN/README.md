ACL Inter-VLAN Routing Lab

Topology

Sales1: connected to switch port Fa0/1
Sales2: connected to switch port Fa0/2
HR1: connected to switch port Fa0/3
HR2: connected to switch port Fa0/4
Router (G0/0/0) connected to switch port Fa0/5 (Trunk)
Switch

The goal is to control communication between VLANs using an ACL applied on the router.

Sales1 & Sales2 connected to VLAN 10 (Sales)
HR1 & HR2 connected to VLAN 20 (HR)

What I Learned:

ACLs (Access Control Lists) allows for controlling and filtering network traffic.

ACLs process rules from top to bottom (first match wins).

ICMP has different message types:

echo = ping request

echo-reply = ping response

To block one-way ping, we must block ICMP echo but allow ICMP echo-reply.

Applying an ACL to an interface using “ip access-group” filters traffic entering or leaving that interface.

ACLs are placed where the traffic arrives (inbound) or exits (outbound) depending on the design.

By applying the ACL on VLAN 20’s router subinterface (HR), we can block HR from pinging Sales while still allowing the opposite direction.

Router Configuration:

enable (to enter privileged mode)
configure terminal (enter global config mode)

Remove old ACL (clean start):

no access-list 100 ← delete any previous ACL entries

Create new ACL rules:

access-list 100 deny icmp 192.168.20.0 0.0.0.255 192.168.10.0 0.0.0.255 echo ← block HR from pinging Sales (deny echo request)

access-list 100 permit icmp 192.168.20.0 0.0.0.255 192.168.10.0 0.0.0.255 echo-reply ← allow HR to send ping replies back to Sales

access-list 100 permit ip any any ← allow all other traffic normally

Apply ACL to HR subinterface:

interface gigabitEthernet0/0/0.20 ← enter VLAN 20 subinterface
ip access-group 100 in ← apply ACL inbound to filter HR traffic
exit

now, HR cannot ping Sales, but Sales can ping HR because the ACL only blocks ICMP echo (requests) and still allows ICMP echo-reply (responses).