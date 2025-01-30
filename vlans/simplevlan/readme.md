Simple VLAN design, where the entire network is configured on a single central switch.

The sectors were designated into 4 VLANs, namely: Financial, HR, Production and Administration.

Note that no sector has access to anything else, except the administration sector, which is responsible for managing the network and, therefore, must have full access to all stations.

The commands used in the Switch CLI are described right below.

Creating VLANs:
```bat
enable
configure terminal
vlan 10
  name Financeiro
vlan 20
  name RH
vlan 30
  name TI
vlan 100
  name Servidor
exit
```
Configuring VLANs:
```bat
interface vlan 10
  ip address 192.168.10.1 255.255.255.0
interface vlan 20
  ip address 192.168.20.1 255.255.255.0
interface vlan 30
  ip address 192.168.30.1 255.255.255.0
interface vlan 100
  ip address 192.168.100.1 255.255.255.0
no shutdown
```
assigning ports to VLANs:
```bat
interface range fa0/1-5
  switchport mode access
  switchport access vlan 10
interface range fa0/6-10
  switchport mode access
  switchport access vlan 20
interface range fa0/11-15
  switchport mode access
  switchport access vlan 30
interface fa0/24
  switchport mode access
  switchport access vlan 100
```
Configuring ACLs:
```bat
access-list 101 permit ip 192.168.10.0 0.0.0.255 192.168.100.0 0.0.0.255
access-list 101 permit ip 192.168.20.0 0.0.0.255 192.168.100.0 0.0.0.255
access-list 101 permit ip 192.168.30.0 0.0.0.255 192.168.100.0 0.0.0.255
access-list 101 deny ip 192.168.10.0 0.0.0.255 192.168.20.0 0.0.0.255
access-list 101 deny ip 192.168.10.0 0.0.0.255 192.168.30.0 0.0.0.255
access-list 101 deny ip 192.168.20.0 0.0.0.255 192.168.30.0 0.0.0.255
```
Applying ACLs:
```bat
interface vlan 10
  ip access-group 101 in
interface vlan 20
  ip access-group 101 in
interface vlan 30
  ip access-group 101 in
ip routing
```


