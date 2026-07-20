ip default-gateway 192.168.10.1
interface 1
   no power-over-ethernet
   exit
interface 2
   no power-over-ethernet
   exit
interface 4
   no power-over-ethernet
   exit
interface 5
   disable
   no power-over-ethernet
   exit
interface 6
   disable
   no power-over-ethernet
   exit
interface 7
   disable
   no power-over-ethernet
   exit
interface 8
   disable
   no power-over-ethernet
   exit
interface 10
   disable
   exit
snmp-server community "public"
snmp-server contact "Henry"
vlan 1
   name "DEFAULT_VLAN"
   no untagged 1-10
   no ip address
   exit
vlan 10
   name "MGMT"
   tagged 1
   ip address 192.168.10.2 255.255.255.0
   exit
vlan 20
   name "LAB"
   untagged 2
   tagged 1
   no ip address
   exit
vlan 30
   name "PC"
   untagged 4
   tagged 1
   no ip address
   exit
vlan 40
   name "GENERAL"
   untagged 5-10
   tagged 1,3
   no ip address
   exit
vlan 50
   name "IOT"
   tagged 1,3
   no ip address
   exit
vlan 100
   name "VPN"
   no ip address
   exit

