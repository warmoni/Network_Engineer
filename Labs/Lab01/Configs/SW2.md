```
!
! Last configuration change at 09:38:19 MSK Wed Sep 20 2023
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
service password-encryption
service compress-config
!
hostname SW2
!
boot-start-marker
boot-end-marker
!
!
enable secret 8 $8$aux2ioJaRrlxP1$Fay9PzBQ0jz3x7bZ/86fX8FPRni1783dVcIA544fbIs
enable password 7 094F471A1A0A
!
no aaa new-model
clock timezone MSK 3 0
!
!
!
!
!
!
!
!
ip dhcp snooping vlan 20
no ip domain-lookup
ip cef
no ipv6 cef
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
vlan internal allocation policy ascending
!
! 
!
!
!
!
!
!
!
!
!
!
!
interface Ethernet0/0
 no shutdown
 description sw2-to-r1
 switchport trunk allowed vlan 10,20,30
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 777
 switchport mode trunk
 ip dhcp snooping trust
!
interface Ethernet0/1
 no shutdown
 switchport trunk allowed vlan 10,30
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 777
 switchport mode trunk
!
interface Ethernet0/2
 no shutdown
 switchport access vlan 20
 switchport mode access
 spanning-tree portfast
!
interface Ethernet0/3
 no shutdown
!
interface Ethernet1/0
 no shutdown
!
interface Ethernet1/1
 no shutdown
!
interface Ethernet1/2
 no shutdown
!
interface Ethernet1/3
 no shutdown
!
interface Ethernet2/0
 no shutdown
!
interface Ethernet2/1
 no shutdown
!
interface Ethernet2/2
 no shutdown
!
interface Ethernet2/3
 no shutdown
!
interface Vlan30
 no shutdown
 ip address 192.168.30.3 255.255.255.0
!
ip forward-protocol nd
!
no ip http server
no ip http secure-server
!
ip route 0.0.0.0 0.0.0.0 192.168.30.1
!
!
!
!
!
control-plane
!
banner login 
$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$
$$          All activity is subject to monitoring.         $$
$$      Any UNAUTHORIZED access or use is PROHIBITED,      $$
$$              and may result in PROSECUTION.             $$
$$                      << SW2 >>                          $$
$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$


!
line con 0
 password 7 13061E010803
 logging synchronous
line aux 0
line vty 0 4
 password 7 0822455D0A16
 login
!
!
end
```