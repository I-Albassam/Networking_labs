# VLAN basics lab

# Topology
PC0: connected to switch's port fa0/1
PC1: connected to switch's port fa0/2
PC2: connected to switch's port fa0/3
PC3: connected to switch's port fa0/4
Switch

The goal is to seperate the traffic on a single switch using VLAN

PC1 & PC2 → VLAN 10 (Group A)
PC3 & PC4 → VLAN 20 (Group B)

# Switch Configuration:

enable ← enter privileged mode
configure terminal ← enter global config mode

vlan 10 ← create VLAN 10
name group_A

vlan 20 ← create VLAN 20
name group_B

interface range fa0/1 - 2 ← select ports for PC1 and PC2
switchport mode access ← set ports as access ports
switchport access vlan 10 ← assign them to VLAN 10

interface range fa0/3 - 4 ← select ports for PC3 and PC4
switchport mode access ← access port (end devices)
switchport access vlan 20 ← assign them to VLAN 20

Now only PCs within the same VLAN can communicate with each other.