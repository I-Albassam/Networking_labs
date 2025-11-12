# Inter-VLAN Routing Lab

Topology

Sales1: connected to switch port Fa0/1
Sales2: connected to switch port Fa0/2
HR1: connected to switch port Fa0/3
HR2: connected to switch port Fa0/4
Router (G0/0/0) connected to switch port Fa0/5 (Trunk)
Switch

The goal is to allow communication between VLANs using a router configured with subinterfaces (Router-on-a-Stick).

Sales1 & Sales2 connected to VLAN 10 (Sales)
HR1 & HR2 connected to VLAN 20 (HR)

# What I Learned:

VLANs separate traffic into different logical networks for organization and security.

Devices in different VLANs cannot communicate unless a Layer 3 device (router) connects them.

A trunk link carries multiple VLANs between switch and router using 802.1Q tagging.

The router uses subinterfaces, each with a VLAN ID and IP address (gateway for that VLAN).

Inter-VLAN Routing allows controlled communication between VLANs while maintaining separation.

Switch Configuration:

# Same steps as basic VLAN lab:

enable (to enter privileged mode)
configure terminal (enter global config mode)

vlan 10 ← create VLAN 10
name Sales

vlan 20 ← create VLAN 20
name HR

interface range fa0/1 - 2 ← select ports for Sales PCs
switchport mode access ← make ports access-only
switchport access vlan 10 ← assign to VLAN 10

interface range fa0/3 - 4 ← select ports for HR PCs
switchport mode access ← make ports access-only
switchport access vlan 20 ← assign to VLAN 20

# New steps for this lab:

interface fa0/5 ← select port connected to router
switchport mode trunk ← enable trunking (carry multiple VLANs)
switchport trunk allowed vlan 10,20 ← allow VLAN 10 and 20 traffic
no shutdown ← activate port


Router Configuration:

interface gigabitEthernet0/0/0 ← select physical interface
no shutdown ← enable it

interface gigabitEthernet0/0/0.10 ← create subinterface for VLAN 10
encapsulation dot1Q 10 ← tag traffic for VLAN 10
ip address 192.168.10.1 255.255.255.0 ← assign gateway IP for VLAN 10

interface gigabitEthernet0/0/0.20 ← create subinterface for VLAN 20
encapsulation dot1Q 20 ← tag traffic for VLAN 20
ip address 192.168.20.1 255.255.255.0 ← assign gateway IP for VLAN 20

now, devices on different VLAN's can communicate with each other using the subinterfaces made in the router.