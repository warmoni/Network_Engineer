```
!
! Last configuration change at 11:55:48 MSK Wed Sep 20 2023
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
service password-encryption
service compress-config
!
hostname SW1
!
boot-start-marker
boot-end-marker
!
!
enable secret 8 $8$qHIx.zuDOKotHn$ZDHChDj7iL.WxILTp2u0WLy0vlyDq01ALQRoX.irNiE
enable password 7 1511021F0725
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
ip dhcp snooping vlan 10
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
 description sw1-to-sw2
 switchport trunk allowed vlan 10,30
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 777
 switchport mode trunk
 ip dhcp snooping trust
!
interface Ethernet0/1
 no shutdown
 switchport access vlan 10
 switchport mode access
 spanning-tree portfast
!
interface Ethernet0/2
 no shutdown
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
 ip address 192.168.30.2 255.255.255.0
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
$$                      << SW1 >>                          $$
$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$


!
line con 0
 password 7 070C285F4D06
 logging synchronous
line aux 0
line vty 0 4
 password 7 02050D480809
 login
!
!
end
```