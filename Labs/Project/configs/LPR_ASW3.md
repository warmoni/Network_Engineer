```

!
! Last configuration change at 14:14:36 MSK Wed May 8 2024
! NVRAM config last updated at 08:02:54 MSK Fri May 10 2024
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
!
hostname LPR_ASW3
!
boot-start-marker
boot-end-marker
!
!
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
 ip address 172.16.2.250 255.255.255.255
 ip ospf 110 area 0
!
interface Ethernet0/0
 no shutdown
 description To-LPR_DSW1
 no switchport
 ip address 172.16.2.14 255.255.255.252
 ip ospf network point-to-point
 ip ospf 110 area 0
 duplex auto
!
interface Ethernet0/1
 no shutdown
 description To-LPR_ASW2
 no switchport
 ip address 172.16.2.21 255.255.255.252
 ip ospf network point-to-point
 ip ospf 110 area 0
 duplex auto
!
interface Ethernet0/2
 no shutdown
 switchport access vlan 3
 switchport mode access
 spanning-tree portfast edge
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
interface Vlan3
 no shutdown
 ip address 192.168.2.33 255.255.255.224
 ip helper-address 172.16.2.253 
 ip ospf 110 area 0
!
router ospf 110
 router-id 5.5.5.5
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
ntp update-calendar
ntp server 172.16.2.253
!
end

```

[Назад](readme.md)