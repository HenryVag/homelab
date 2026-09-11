```
ip default-gateway 192.168.1.1
interface 1
   no power-over-ethernet
   energy-efficient-ethernet
   exit
interface 2
   disable
   no power-over-ethernet
   energy-efficient-ethernet
   exit
interface 3
   disable
   no power-over-ethernet
   energy-efficient-ethernet
   exit
interface 4
   disable
   no power-over-ethernet
   energy-efficient-ethernet
   exit
interface 5
   energy-efficient-ethernet
   exit
interface 6
   disable
   no power-over-ethernet
   energy-efficient-ethernet
   exit
interface 7
   disable
   no power-over-ethernet
   energy-efficient-ethernet
   exit
interface 8
   disable
   no power-over-ethernet
   energy-efficient-ethernet
   exit
interface 9
   disable
   exit
interface 10
   energy-efficient-ethernet
   exit
snmp-server community "public"
snmp-server contact "Henry"
vlan 1
   name "DEFAULT_VLAN"
   no untagged 1,5
   untagged 2-4,6-9
   tagged 10
   no ip address
   exit
vlan 10
   name "MGMT"
   untagged 1
   tagged 5,10
   ip address 192.168.10.20 255.255.255.0
   exit
vlan 40
   name "GENERAL"
   tagged 5,10
   no ip address
   exit
vlan 50
   name "IOT"
   tagged 5,10
   no ip address
   exit
spanning-tree

```
