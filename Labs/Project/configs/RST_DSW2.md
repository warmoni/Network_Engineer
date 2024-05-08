```

!
! Last configuration change at 14:14:22 MSK Wed May 8 2024
! NVRAM config last updated at 08:03:03 MSK Fri May 10 2024
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
!
hostname RST_DSW2
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
 ip address 172.16.1.251 255.255.255.255
 ip ospf 110 area 0
!
interface Port-channel1
 no shutdown
 no switchport
 ip address 172.16.1.26 255.255.255.252
 ip ospf network point-to-point
 ip ospf 110 area 0
!
interface Ethernet0/0
 no shutdown
 description To-RST_R2
 no switchport
 ip address 172.16.1.18 255.255.255.252
 ip ospf network point-to-point
 ip ospf 110 area 0
 duplex auto
!
interface Ethernet0/1
 no shutdown
 description To-RST_R1
 no switchport
 ip address 172.16.1.14 255.255.255.252
 ip ospf network point-to-point
 ip ospf 110 area 0
 duplex auto
!
interface Ethernet0/2
 no shutdown
 no switchport
 no ip address
 duplex auto
 channel-group 1 mode active
!
interface Ethernet0/3
 no shutdown
 no switchport
 no ip address
 duplex auto
 channel-group 1 mode active
!
interface Ethernet1/0
 no shutdown
 description To-RST_ASW1
 no switchport
 ip address 172.16.1.41 255.255.255.252
 ip ospf network point-to-point
 ip ospf 110 area 1
 duplex auto
!
interface Ethernet1/1
 no shutdown
 description To-RST_ASW2
 no switchport
 ip address 172.16.1.45 255.255.255.252
 ip ospf network point-to-point
 ip ospf 110 area 1
 duplex auto
!
interface Ethernet1/2
 no shutdown
 description To-RST_ASW3
 no switchport
 ip address 172.16.1.49 255.255.255.252
 ip ospf network point-to-point
 ip ospf 110 area 1
 duplex auto
!
interface Ethernet1/3
 no shutdown
!
router ospf 110
 router-id 4.4.4.4
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
ntp update-calendar
ntp server 172.16.1.6
!
end

```

[Назад](readme.md)