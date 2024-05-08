```

!
! Last configuration change at 14:14:20 MSK Wed May 8 2024
! NVRAM config last updated at 08:03:01 MSK Fri May 10 2024
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
!
hostname RST_ASW2
!
boot-start-marker
boot-end-marker
!
!
no logging console
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
ip cef
ip cef load-sharing algorithm original
no ipv6 cef
!
!
!
spanning-tree mode rapid-pvst
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
!
interface Loopback1
 no shutdown
 ip address 172.16.1.249 255.255.255.255
 ip ospf 110 area 1
!
interface Ethernet0/0
 no shutdown
 description To-RST_DSW1
 no switchport
 ip address 172.16.1.34 255.255.255.252
 ip helper-address 172.16.1.6 
 ip ospf network point-to-point
 ip ospf 110 area 1
 duplex auto
!
interface Ethernet0/1
 no shutdown
 description To-RST_DSW2
 no switchport
 ip address 172.16.1.46 255.255.255.252
 ip ospf network point-to-point
 ip ospf 110 area 1
 duplex auto
!
interface Ethernet0/2
 no shutdown
 switchport access vlan 4
 switchport mode access
 spanning-tree portfast edge
!
interface Ethernet0/3
 no shutdown
 switchport access vlan 5
 switchport mode access
 spanning-tree portfast edge
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
interface Vlan4
 no shutdown
 ip address 192.168.1.65 255.255.255.224
 ip helper-address 172.16.1.247 
 ip ospf network point-to-point
 ip ospf 110 area 1
!
interface Vlan5
 no shutdown
 ip address 192.168.1.97 255.255.255.224
 ip helper-address 172.16.1.247 
 ip ospf network point-to-point
 ip ospf 110 area 1
!
router ospf 110
 router-id 6.6.6.6
 area 1 stub
!
ip forward-protocol nd
!
no ip http server
no ip http secure-server
!
!
!
!
!
!
control-plane
!
!
line con 0
 logging synchronous
line aux 0
line vty 0 4
 login
!
ntp server 172.16.1.6
!
end

```

[Назад](readme.md)